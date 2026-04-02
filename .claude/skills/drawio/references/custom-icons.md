# Diagram Shape Styles

よく使うアクター・要素の組み込みシェイプスタイル集。Base64 エンコード不要。

## 人間 / ユーザー (actor)

UML アクター（スティックフィギュア）。

```xml
<mxCell id="human1" value="ユーザー"
        style="shape=actor;whiteSpace=wrap;"
        vertex="1" parent="1">
  <mxGeometry x="100" y="100" width="40" height="60" as="geometry"/>
</mxCell>
```

## AI エージェント (ellipse)

楕円形に紫色を使うとエージェントらしく見える。

```xml
<mxCell id="ai1" value="AI Agent"
        style="ellipse;whiteSpace=wrap;fillColor=#7C3AED;strokeColor=#5B21B6;fontColor=#ffffff;"
        vertex="1" parent="1">
  <mxGeometry x="100" y="100" width="80" height="80" as="geometry"/>
</mxCell>
```

## リポジトリ / GitHub (cylinder3)

シリンダーはリポジトリ・ストレージの表現に適している。

```xml
<mxCell id="repo1" value="Repository"
        style="shape=cylinder3;whiteSpace=wrap;fillColor=#24292F;strokeColor=#000000;fontColor=#ffffff;"
        vertex="1" parent="1">
  <mxGeometry x="100" y="100" width="60" height="70" as="geometry"/>
</mxCell>
```

## CI/CD / GitHub Actions (process)

プロセス形状（平行四辺形の変形）はワークフローの表現に適している。

```xml
<mxCell id="ci1" value="CI/CD"
        style="shape=process;whiteSpace=wrap;fillColor=#2088FF;strokeColor=#1A6FCC;fontColor=#ffffff;"
        vertex="1" parent="1">
  <mxGeometry x="100" y="100" width="120" height="60" as="geometry"/>
</mxCell>
```

## Wiki / ドキュメント (document)

波形の底辺を持つドキュメント形状。

```xml
<mxCell id="wiki1" value="Wiki"
        style="shape=document;whiteSpace=wrap;fillColor=#1B7D3A;strokeColor=#155C2B;fontColor=#ffffff;"
        vertex="1" parent="1">
  <mxGeometry x="100" y="100" width="120" height="60" as="geometry"/>
</mxCell>
```

## その他の汎用シェイプ

| 用途 | shape 値 | 備考 |
|------|----------|------|
| データベース | `shape=cylinder3` | |
| 判断分岐 | `rhombus` | ダイヤモンド形 |
| 開始/終了 | `ellipse` | 端点 |
| クラウド | `shape=mxgraph.cisco.sites.cloud` | |
| サーバー | `shape=mxgraph.network.server` | |
| スマートフォン | `shape=mxgraph.android.phone2` | |
