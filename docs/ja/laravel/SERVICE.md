# サービス

このドキュメントでは、Laravel アプリケーションにおけるサービス層の設計に関するガイドラインを定めます。

## クエリの取得方法

- **[厳守]** `->get()`（全件取得）と `->paginate()`（ページネーション取得）のどちらを使うかは、呼び出し側（コントローラー等）が判断する。リポジトリ/サービス層で「ページネーションする版」「しない版」を別々のメソッドとして用意せず、単一のクエリ構築メソッドが `Builder` を返す（`Collection` や `LengthAwarePaginator` に確定させない）ようにし、呼び出し側でその `Builder` に対して `->get()` または `->paginate()` を呼ぶ。本ルールは [リポジトリ](./REPOSITORY.md) 層にも同様に適用される。

```php
// ❌ 誤った実装：get() 版と paginate() 版を別メソッドとして重複させる
interface FooRepositoryInterface
{
    public function findByParams(array $params): Collection;
    public function findByParamsWithPaginator(array $params, int $limit): LengthAwarePaginator;
    public function buildQueryByParams(array $params): Builder;
}

class FooRepository implements FooRepositoryInterface
{
    public function findByParams(array $params): Collection
    {
        return $this->buildQueryByParams($params)->get();
    }

    public function findByParamsWithPaginator(array $params, int $limit): LengthAwarePaginator
    {
        return $this->buildQueryByParams($params)->paginate($limit);
    }

    public function buildQueryByParams(array $params): Builder
    {
        // ...
    }
}

// ✅ 正しい実装：クエリ構築のみを提供し、呼び出し側が get()/paginate() を判断する
interface FooRepositoryInterface
{
    public function buildQueryByParams(array $params): Builder;
}

class FooRepository implements FooRepositoryInterface
{
    public function buildQueryByParams(array $params): Builder
    {
        // ...
    }
}

// 呼び出し側（コントローラー等）で判断する
$query = $repository->buildQueryByParams($params);
$items = $query->paginate($perPage); // または $query->get();
```

理由:

- `->get()` 版と `->paginate()` 版は実質的に「クエリを組み立てた後に何を呼ぶか」だけが違い、クエリ構築ロジック自体は同一。これを別メソッドとして重複させると、条件を追加・変更する際に両方のメソッドを同期して直す必要が生じ、片方だけ直し忘れるバグの温床になる。
- ページネーションの要否・件数（`per_page`）は本質的にリクエスト（画面や API の用途）に紐づく関心事であり、データ取得ロジックを持つリポジトリ/サービス層の関心事ではない。レイヤーの責務を混ぜないために、呼び出し側に判断を委ねる。
