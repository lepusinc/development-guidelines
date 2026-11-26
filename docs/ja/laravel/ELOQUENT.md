# Eloquent モデル

このドキュメントでは、Laravel アプリケーションにおける Eloquent モデルの設計・実装に関するガイドラインを定めます。

## 1. 型安全性

- **[要修正]** `type` や `status` のような、DB カラムに特定の文字列が保存される値は、専用の Enum を作成しキャストを設定する。

## 2. クエリビルダー

- **[厳守]** Builder（およびスコープ）は担当するテーブルのクエリビルドのみに限定する。リレーションしているテーブルへのクエリが必要な場合は、そのテーブルの Builder に委譲する。
- **[要修正]** モデルクラスにスコープは定義せず、専用の Builder クラスを作成し Builder に定義する。
- **[厳守]** クエリの組み立て（条件・集計・並び順など）は、モデルのメソッドに書かず、Builder に置く。スコープに限らず、モデルの普通のメソッドの中でもクエリを組み立てない。ただし、すでにスコープを採用しているモデルでは、そのモデルのテーブルへの条件・並び順をスコープとして定義してよい（このセクションの最後のルールを参照）。
- **[厳守]** 条件は、その条件をかけるテーブルのモデルの Builder（すでにスコープを採用しているモデルでは、そのモデルのスコープでもよい）に置く。親のモデルに、子のテーブルへの条件を書かない（例：子の `children` テーブルへの条件は、親の `ParentModel` ではなく、子の `ChildModel` の Builder に置く）。
- **[要修正]** 親から子を絞り込むときは、リレーション越しにメソッドチェーンで呼ぶ。条件の組み合わせは、意味のある名前の Builder のメソッドにまとめる。

```php
// 悪い例：親のモデルのメソッドで、子のテーブルへの条件を組み立てている
class ParentModel extends Model
{
    public function activeChildren(): HasMany
    {
        return $this->children()
            ->where('status', ChildStatus::Active)
            ->whereNull('archived_at');
    }
}

// 良い例：子のテーブルへの条件は、子のモデルの Builder にまとめる
class ChildModelBuilder extends Builder
{
    public function active(): static
    {
        return $this->where('status', ChildStatus::Active)
            ->whereNull('archived_at');
    }
}

// 子のモデルは、newEloquentBuilder() でその Builder を返す
class ChildModel extends Model
{
    public function newEloquentBuilder($query): ChildModelBuilder
    {
        return new ChildModelBuilder($query);
    }
}

// 呼び出し側：リレーション越しにメソッドチェーンで呼ぶ
$children = $parent->children()->active()->get();
```

- **[要修正]** 条件や並び順は、原則として、その条件をかけるテーブルのモデルの Builder にメソッドとして定義して使う。ただし、そのモデルがすでにスコープを採用している場合は、「モデルクラスにスコープは定義しない」ルールの例外として、すでにあるスコープを使い、新しい条件もスコープとして定義してよい。カスタムビルダーへの移行は、リファクタリングとして検討する。どちらの場合も、すでにある条件（Builder のメソッドやスコープ）と同じ意味の条件を、`where()` や `orderBy()` で書き直したり、別の場所に定義し直したりしない。`whereHas()` などのクロージャの中でも同じで、関係先のモデルの Builder のメソッド（またはスコープ）を使う。

```php
// 悪い例：スコープや Builder のメソッドがある条件を、where() で書き直している
ParentModel::query()
    ->where('status', ParentStatus::Active)
    ->whereHas('children', fn ($query) => $query
        ->where('status', ChildStatus::Active)
        ->whereNull('archived_at'));

// 良い例：ParentModel にすでに定義されているスコープ（scopeActive()）と、
// ChildModel の Builder のメソッド（active()）を使う
ParentModel::query()
    ->active()
    ->whereHas('children', fn ($query) => $query->active());
```

## 3. コレクション

- **[要修正]** 複数のモデルを使う独自処理を実装する場合、独自コレクションを作成し独自コレクションに実装する。ただし、コレクション内のモデルだけで完結する処理領域に限る（他のサービスクラスなどへの委譲は含めてはいけない。リレーション定義しているモデルや Eloquent コレクションへの委譲は可）。

## 4. 責務と設計

- **[厳守]** モデルおよび独自コレクションでサービスクラスを use することは絶対に禁止する。ORM の領域を逸脱するため。
- **[厳守]** モデルの stateful な値は外部から直接値を設定できないようにし、状態変更の専用メソッドを定義する。汎用の考え方は [ドメインモデル（エンティティ）](../DOMAIN_MODEL.md) にある。Eloquent では、次のようにする。
  - 状態に関わる属性（`status` などのステータス、結果、有効フラグ、状態の変更に付随する日時など）は `$fillable` に含めない。`create()` / `fill()` / `update()` で設定させない。
  - `$fillable` から外しても `$model->status = ...` の代入は防げない。モデルの外（サービス、コントローラーなど）から、状態に関わる属性へ代入しない。
  - 状態の変更に付随する値（例：`requested_at`）は、状態を変える専用メソッドの引数で受け取る。
  - クエリビルダーでの一括更新（`update()`）で状態に関わる列を変える場合も、モデル・独自コレクション・Builder の専用メソッドの中に閉じ込める。一括更新ではモデルイベント（`updating` / `updated` など）が発火しないため、Observer の副作用（[7. モデルイベント](#7-モデルイベント)）や監査ログは実行されない。これらが必要な状態の変更では一括更新を使わず、モデルごとに専用メソッドで変更する。

```php
// 悪い例：サービスから状態と付随する値を直接設定している
$batch->status = BatchStatus::InProgress;
$batch->requested_at = now();
$batch->save();

// 良い例：モデルの専用メソッドが、移れるかの確認と付随する値の記録を行う
$batch->markRequested(now());
```

## 5. セキュリティ

- **[厳守]** `$guarded = []` は使用禁止。`$fillable` でマスアサインメントの対象属性をホワイトリストで明示する。
- **[厳守]** 連番の主キーを RouteKey に使用しない。連番 ID を URL に露出させると列挙攻撃のリスクがあるため、`getRouteKeyName()` で UUID などの非推測な値を Route Key として使用する。
- **[要修正]** シリアライズ時に漏洩させたくない属性（パスワードなど）は `$hidden` で除外する。

## 6. パフォーマンス

- **[要修正]** リレーションを参照する際は `with()` でイーガーロードし、N+1 問題を防ぐ。
- **[要修正]** `$with` によるデフォルトイーガーロードは原則設定しない。`with()` と異なり、`$with` はモデル取得のたびに暗黙的なイーガーロードを実行し、各リレーションに対して `WHERE IN` による追加クエリが発行される。リレーションデータが不要な場合でも常にクエリが走るため、不要なオーバーヘッドになる。

## 7. モデルイベント

- **[要修正]** モデルのライフサイクルに紐づくサイドエフェクト（通知送信、ログ記録など）は Observer に集約し、モデル内には直書きしない。

## 8. 設定

- **[厳守]** UUID など非連番の主キーを使用する場合は `$incrementing = false` と `$keyType = 'string'` を明示する。
- **[要修正]** 論理削除が必要なモデルは `SoftDeletes` トレイトを使用する。

## 9. リレーション

- **[厳守]** ドメイン固有の命名規則（アルファベット順でない）に従った中間テーブルを持つ `belongsToMany()` を定義する際は、第2引数に中間テーブル名を明示する。Laravel はデフォルトでテーブル名をアルファベット順に解決するため、省略すると誤ったテーブルが参照される。

```php
// Laravel のデフォルト解決はアルファベット順: http_communication_payment
// ドメイン固有の順序には明示的な宣言が必要:
public function httpCommunications(): BelongsToMany
{
    return $this->belongsToMany(HttpCommunication::class, 'payment_http_communication');
}
```
