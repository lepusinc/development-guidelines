# 認可（Authorization）

このドキュメントでは、Laravel アプリケーションにおける認可のガイドラインを定めます。

## 基本方針

- **[厳守]** リソースへのアクセス制御は Laravel Policy で一元管理する。コントローラのプライベートメソッドや Blade テンプレートの `@php` ブロックに認可ロジックを書かない。
- **[厳守]** コントローラでは `$this->authorize()` を使って Policy を呼び出す。`abort()` を直接呼ぶプライベートメソッド（`abortIfXxx()` 等）を作らない。
- **[厳守]** Blade テンプレートでの表示制御は `@can` / `@cannot` ディレクティブを使う。`@php` ブロックで権限変数を計算して `@if` で分岐しない。
- **[推奨]** ルートレベルでの認可は `->middleware('can:action,model')` と組み合わせて多重防御する。

## Policy の設計

- **[推奨]** Policy のメソッド名はアクション名（`view`, `create`, `update`, `delete`）または業務上の意味のある名前（`viewContracts`, `updateContracts`）にする。
- **[推奨]** 複合条件（フィーチャーフラグ＋モデル属性など）も Policy にまとめる。「ユーザーがこのリソースに対してこの操作を行えるか」という問いに答える責務であり、設定値によるアクセス制御も含んでよい。

## AuthServiceProvider への登録

```php
protected $policies = [
    Fund::class => FundPolicy::class,
];
```

## 実装例

```php
// ✅ 正しい実装：Policy にアクセス制御を集約する

// app/Policies/FundPolicy.php
public function viewContracts(?Admin $admin, Fund $fund): bool
{
    return !CommonHelper::usageRestricted() && $fund->isElectronicContract();
}

public function updateContracts(Admin $admin, Fund $fund): bool
{
    return $this->viewContracts($admin, $fund)
        && CommonHelper::isContractPDFFeatureEnabled();
}

// コントローラ
public function show(Fund $fund): View
{
    $this->authorize('viewContracts', $fund);
    // ...
}

public function update(WriteRequest $request, Fund $fund): RedirectResponse
{
    $this->authorize('updateContracts', $fund);
    // ...
}

// Blade テンプレート
@can('updateContracts', $fund)
    @include('admin.fund.partials.edit-fund-contracts')
@endcan

@can('viewContracts', $fund)
    @cannot('updateContracts', $fund)
        <div class="bgc-white p-20 bd mB-20">
            契約書PDF機能が無効のため、この画面で契約書のアップロードは行えません。
        </div>
    @endcannot
@endcan
```

```php
// ❌ 誤った実装：コントローラのプライベートメソッドで制御する
private function abortIfContractManagementUnavailable(Fund $fund): void
{
    if (CommonHelper::usageRestricted() || !$fund->isElectronicContract()) {
        abort(404);
    }
}
```

```blade
{{-- ❌ 誤った実装：Blade の @php で権限変数を計算する --}}
@php
    $canManagePdfContracts = !CommonHelper::usageRestricted()
        && $fund->isElectronicContract()
        && CommonHelper::isContractPDFFeatureEnabled();
@endphp
@if ($canManagePdfContracts)
    ...
@endif
```
