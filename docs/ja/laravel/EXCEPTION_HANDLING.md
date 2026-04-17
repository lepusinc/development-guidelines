# 例外ハンドリング / ロギング

このドキュメントでは、Laravel アプリケーションにおける例外ハンドリングとロギングに関するガイドラインを定めます。

## 基本方針

- **[厳守]** 例外時のレスポンス・コマンドの出力制御・ロギングは `app/Exceptions/Handler.php` で一元管理する（HTTP は `render()`、コンソールは `renderForConsole()`、ロギングは `report()`）。アプリケーションコード内でこれらを目的とした例外キャッチを行わない。
- ただし、以下の目的での例外キャッチは許容する：
  - トランザクションのロールバック
  - ファイルやリソースのクローズ
  - 想定された例外に対して `false` や代替値を返す（ビジネスロジック上の分岐）
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

## イベントリスナーの例外ハンドリング

- **[厳守]** イベントリスナー内で発生した例外は、リスナー自身の `handle()` メソッド内でキャッチ・処理する。呼び出し元（コントローラ・サービス）が `event()` を try/catch で囲んでリスナーの失敗を処理してはならない。
- リスナーが `ShouldQueue` を実装している場合、リスナー内の例外は dispatch 元に伝播しないため、呼び出し元での try/catch はそもそも機能しない。キュー済みリスナーの例外ハンドリングは `failed()` メソッドで行う。

```php
// ✅ 正しい実装：リスナー自身が例外をハンドリングする
class LogActionEventListener
{
    public function handle(UpdateFundEvent $event): void
    {
        try {
            $this->addAction->addLoggableAction(...);
        } catch (Throwable $e) {
            report($e);
            Log::error('Audit log failed', ['exception' => $e]);
        }
    }
}

// ✅ 正しい実装：キュー済みリスナーは failed() を使う
class SendNotificationListener implements ShouldQueue
{
    public function handle(FundUpdated $event): void { ... }

    public function failed(FundUpdated $event, Throwable $exception): void
    {
        report($exception);
        Log::error('Notification job failed', ['exception' => $exception]);
    }
}

// ❌ 誤った実装：呼び出し元がリスナーの例外を catch している
try {
    event(new UpdateFundEvent(...));
} catch (Throwable $e) {
    report($e);
    Log::error('event dispatch failed', ['exception' => $e]);
}
```
