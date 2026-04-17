# 例外ハンドリング / ロギング

このドキュメントでは、Laravel アプリケーションにおける例外ハンドリングとロギングに関するガイドラインを定めます。

## 基本方針

- **[厳守]** 例外時のレスポンス・コマンドの出力制御・ロギングは `app/Exceptions/Handler.php` で一元管理する（HTTP は `render()`、コンソールは `renderForConsole()`、ロギングは `report()`）。アプリケーションコード内でこれらを目的とした例外キャッチを行わない。
- **[厳守]** アプリケーション層のコードで `Throwable` を catch しない。`Throwable` には `Error` クラス（`TypeError`・`ParseError`・`OutOfMemoryError` 等）が含まれ、これらはプログラミングミスや回復不能なランタイムエラーを示す。すべての `Throwable` のハンドリングはグローバル例外ハンドラ（`App\Exceptions\Handler`）に委ねて一元管理する。アプリケーション層の `try`/`catch` は `Exception` または具体的なサブクラスのみを対象とする。（Laravel が定義するシグネチャ `failed(Event $event, Throwable $exception)` のように `Throwable` を引数で受け取ることは catch ではないので問題ない）
- ただし、以下の目的での例外キャッチは許容する：
  - トランザクションのロールバック
  - ファイルやリソースのクローズ
  - 想定された例外に対して `false` や代替値を返す（ビジネスロジック上の分岐）
  - ユーザー向けメッセージに載せ替えて throw し直す（詳細は「コントローラでの例外ハンドリングパターン」参照）
- **[厳守]** UX を変更する目的のためだけに例外をキャッチして握りつぶすことは絶対に許容しない。レスポンスの制御は Handler で行う。
- **[推奨]** Handler に委譲されていない例外レスポンスが存在する場合、責務境界や例外設計の見直しを検討する。

## 例外クラスの設計

- **[厳守]** 例外のログレベルの変更は `Handler` の `$levels` でマッピングする。マッピングのキーにはクラス名・インターフェース名どちらも使用できる。
- **[推奨]** ドメイン固有のエラーにはカスタム例外クラスを作成し、例外の意味をクラス名で表現する（例：`PaymentFailedException`、`ResourceNotFoundException`）。

```php
protected $levels = [
    ValidationException::class  => LogLevel::INFO,
    WarningLoggable::class      => LogLevel::WARNING,  // インターフェース指定も可
    ExternalApiException::class => LogLevel::ERROR,
];
```

## ログレベルの基準

| レベル | 用途の目安 |
| --- | --- |
| `INFO` | バリデーションエラーなどユーザー起因の想定内エラー |
| `WARNING` | ビジネスルール違反、リトライ可能な外部サービスエラーなど |
| `ERROR` | システム障害、データ不整合など即時対応が必要なもの |
| `CRITICAL` | サービス継続不能レベルの障害 |

## レスポンス

- **[厳守]** HTTP レスポンスへの変換も `Handler` の `render()` または `renderable()` で一元管理する。
- **[厳守]** スタックトレースや内部情報を本番環境のレスポンスに含めない。
- **[要修正]** エラーレスポンスのフォーマット（構造・フィールド名）をアプリケーション全体で統一する。

## コントローラでの例外ハンドリングパターン

コントローラで例外を catch した後の処理として、以下の 2 パターンのみを許容する。

### パターン 1: ユーザー向けメッセージに載せ替えて throw し直す

低レベルの例外をユーザーに見せられるメッセージに差し替え、ユーザー向け例外として throw し直す。Handler が識別して、そのメッセージを含むユーザー向けエラーレスポンスを返す（具体的な HTTP ステータスコードやビューは Handler 側の責務）。元の例外は `previous` に渡してスタックトレースを保持する。

```php
// ✅ 許容：ユーザー向けメッセージに載せ替えて throw し直す
try {
    // ...
} catch (Exception $e) {
    throw new XxxException(__('messages.alerts.try_again_later'), previous: $e);
}

// ❌ 禁止：コントローラで例外をキャッチしてリダイレクトを制御する
try {
    // ...
} catch (Exception $e) {
    return redirect()->back()->with(['warning' => __('messages.alerts.try_again_later')]);
}
```

### パターン 2: サービス層が返り値で成否を表現し、コントローラが if 制御する（別画面へのリダイレクト）

別の画面へリダイレクトする必要がある場合は、例外ではなく返り値で成否を表現し、コントローラはその値で分岐する。返り値の具体的な形（Result オブジェクト・DTO・タプル・enum・`null` とオブジェクトの組み合わせ、など）は実装詳細であり、重要なのは成否を throw ではなく値として運ぶこと。

```php
// ✅ 許容：サービスが成否を表現する値を返し、コントローラが分岐
$result = $updateService->save($fund, $values);
if ($result->failed()) {
    return redirect()->route('admin.fund.other', $fund->id)
        ->with(['warning' => $result->errorMessage()]);
}

// ❌ 禁止：コントローラで例外をキャッチしてリダイレクト先を制御する
try {
    $updateService->save($fund, $values);
} catch (Exception $e) {
    return redirect()->route('admin.fund.other', $fund->id)
        ->with(['warning' => __('messages.alerts.try_again_later')]);
}
```

## イベントリスナーの例外ハンドリング

Laravel 公式ドキュメントが明示的に規定しているのは `ShouldQueue` リスナーの `failed()` パターンのみ。それ以外は本プロジェクトが採用する設計原則として、「`event()` の呼び出し元は、リスナーが同期実行かキュー実行かを意識しない」を置く。同期／非同期はリスナー側の実装詳細であり、後から `ShouldQueue` を付ける／外すといった変更も起こり得る。したがってリスナーの失敗の扱いはリスナー側に閉じる。

- **[厳守]** `ShouldQueue` リスナーは `handle()` から例外を外に伝播させる。失敗時の後処理は `InteractsWithQueue` トレイトを use したうえで `failed(Event $event, Throwable $exception)` メソッドに書く。

  理由: Laravel のキューリトライ機構（`$tries`・`backoff`・`retryUntil`）とリトライ使い果たし時の `failed()` 呼び出しは、例外が `handle()` から伝播することで初めて機能する。`handle()` を try/catch で握りつぶすとこれらがまとめて効かなくなる。参考: [Laravel Events — Handling Failed Jobs](https://laravel.com/docs/12.x/events#handling-failed-jobs)

- **[推奨]** 呼び出し元は `event()` を try/catch で囲まない。失敗処理はリスナー側に閉じる。

  理由: 呼び出し元に例外が伝播するのは同期リスナーの場合だけで、try/catch は全リスナーが同期実行の間しか機能しない。いずれかのリスナーに `implements ShouldQueue` が付いた瞬間、catch 節は無音で効かなくなる。この try/catch はリスナーの実行モード（同期／キュー）という実装詳細に呼び出し元が結合することを意味する。Laravel 公式には明文規定がなく、本プロジェクトの設計原則。

- **[推奨]** 同期リスナーの失敗がメインフローを壊してはならない場合（例: 監査ログ）は、`handle()` 内で catch して `report()` する。リスナーの成功がメインフローに不可欠なら、そのロジックは呼び出し元または専用サービスに移す。

  理由: Laravel は同期リスナーの例外処理について特定のパターンを規定しておらず、設計判断となる。リスナーは疎結合な副作用を載せる仕組みであり、メインフローの成否を左右するロジックを置くと結合度が上がる。

```php
// ✅ 正しい実装：同期の副作用リスナー — handle() 内で catch してメインフローを守る
class LogActionEventListener
{
    public function handle(UpdateFundEvent $event): void
    {
        try {
            $this->addAction->addLoggableAction(...);
        } catch (Exception $e) {
            report($e);
        }
    }
}

// ✅ 正しい実装：キュー済みリスナー — 例外を伝播させ、failed() で後処理する
class SendNotificationListener implements ShouldQueue
{
    use InteractsWithQueue;

    public function handle(FundUpdated $event): void
    {
        // try/catch しない — キューがリトライできるよう例外を伝播させる
        $this->notifier->send(...);
    }

    public function failed(FundUpdated $event, Throwable $exception): void
    {
        report($exception);
    }
}

// ❌ 禁止：キュー済みリスナーの handle() を try/catch で握りつぶす
class SendNotificationListener implements ShouldQueue
{
    public function handle(FundUpdated $event): void
    {
        try {
            $this->notifier->send(...);
        } catch (Throwable $e) {
            report($e); // キューがリトライできず、failed() も呼ばれない
        }
    }
}

// ❌ 非推奨：呼び出し元が event() を try/catch で囲む
// リスナーが同期実行のうちしか機能しない。後からリスナーに `implements ShouldQueue`
// を付けた瞬間、この catch は無音で呼ばれなくなる。呼び出し元がリスナーの実装詳細に結合する。
try {
    event(new UpdateFundEvent(...));
} catch (Throwable $e) {
    report($e);
}
```
