# Azure Shapes Reference

draw.io 組み込みの Azure シェイプライブラリを使用する際のリファレンス。Base64 変換不要。

## 使い方

`style` 属性の `image=` に `img/lib/azure2/` パスを指定するだけで利用可能。

```xml
<mxCell id="5" value="Azure Storage"
        style="shape=image;verticalLabelPosition=bottom;labelBackgroundColor=default;verticalAlign=top;aspect=fixed;imageAspect=0;image=img/lib/azure2/storage/Storage_Accounts.svg;"
        vertex="1" parent="1">
  <mxGeometry x="200" y="100" width="48" height="48" as="geometry"/>
</mxCell>
```

style テンプレート:

```
shape=image;verticalLabelPosition=bottom;labelBackgroundColor=default;verticalAlign=top;aspect=fixed;imageAspect=0;image=img/lib/azure2/{カテゴリ}/{アイコン名}.svg;
```

## よく使うシェイプパス

| リソース | シェイプパス |
|----------|-------------|
| Storage Accounts | `img/lib/azure2/storage/Storage_Accounts.svg` |
| App Service | `img/lib/azure2/app_services/App_Services.svg` |
| Function Apps | `img/lib/azure2/compute/Function_Apps.svg` |
| SQL Database | `img/lib/azure2/databases/SQL_Database.svg` |
| Virtual Machine | `img/lib/azure2/compute/Virtual_Machine.svg` |
| Kubernetes Services | `img/lib/azure2/compute/Kubernetes_Services.svg` |
| Cosmos DB | `img/lib/azure2/databases/Azure_Cosmos_DB.svg` |
| API Management | `img/lib/azure2/app_services/API_Management_Services.svg` |
| Key Vault | `img/lib/azure2/security/Key_Vaults.svg` |
| Container Registry | `img/lib/azure2/containers/Container_Registries.svg` |
| Azure AD | `img/lib/azure2/identity/Azure_Active_Directory.svg` |
| Virtual Network | `img/lib/azure2/networking/Virtual_Networks.svg` |
| Load Balancer | `img/lib/azure2/networking/Load_Balancers.svg` |

## その他のアイコンの探し方

draw.io エディタで「More Shapes → Networking → Azure」を有効化すると、カテゴリ別に全アイコンが表示される。
パスは `img/lib/azure2/{カテゴリ}/{アイコン名}.svg` の形式。

主なカテゴリ: `compute`, `databases`, `storage`, `networking`, `app_services`, `containers`, `security`, `identity`, `general`
