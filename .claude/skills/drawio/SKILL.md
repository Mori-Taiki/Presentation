# Draw.io Diagram Skill

draw.io ネイティブの `.drawio` ファイル（mxGraphModel XML）を生成するスキル。PNG/SVG/PDF へのエクスポートにも対応。

## Key Workflow

1. **mxGraphModel XML を生成**してリクエストされた図を作成する
2. **Write ツールで `.drawio` ファイルに書き込む**
3. **エクスポートが必要な場合**は draw.io CLI を `--embed-diagram` フラグ付きで実行する
4. **結果を開く**

## Critical XML Requirements

- すべての図はこのルート構造が必要: `<mxGraphModel adaptiveColors="auto"><root><mxCell id="0"/><mxCell id="1" parent="0"/></root></mxGraphModel>`
- **エッジには必ず子の geometry 要素を含めること** — 自己クローズタグは不可:
  ```xml
  <mxCell id="e1" edge="1" parent="1" source="a" target="b" style="...">
    <mxGeometry relative="1" as="geometry" />
  </mxCell>
  ```
- **XML コメント（`<!-- -->`）は絶対に含めないこと**
- 特殊文字をエスケープ: `&amp;`, `&lt;`, `&gt;`, `&quot;`

## Export Formats

PNG/SVG/PDF はいずれも図の XML を埋め込んだ状態でエクスポートでき、draw.io で再編集可能。

```bash
drawio -x -f <format> -e -b 10 -o <output> <input.drawio>
```

| フォーマット | 拡張子 | 備考 |
|---|---|---|
| （デフォルト） | `.drawio` | ネイティブ XML、draw.io Desktop 不要 |
| `png` | `.drawio.png` | 埋め込み XML あり、どこでも表示可能 |
| `svg` | `.drawio.svg` | スケーラブル、埋め込み XML あり |
| `pdf` | `.drawio.pdf` | 印刷用、埋め込み XML あり |

## Platform-Specific CLI Paths

| プラットフォーム | パス |
|---|---|
| Windows | `"C:\Program Files\draw.io\draw.io.exe"` |
| macOS | `/Applications/draw.io.app/Contents/MacOS/draw.io` |
| Linux | `drawio`（PATH 経由） |
| WSL2 | `/mnt/c/Program Files/draw.io/draw.io.exe` |

実行時は `drawio` を先に試し、失敗したらプラットフォーム固有のパスにフォールバックする。

## Minimal XML Template

```xml
<mxGraphModel adaptiveColors="auto">
  <root>
    <mxCell id="0"/>
    <mxCell id="1" parent="0"/>
    <mxCell id="2" value="Label" style="rounded=1;whiteSpace=wrap;" vertex="1" parent="1">
      <mxGeometry x="100" y="100" width="120" height="60" as="geometry"/>
    </mxCell>
  </root>
</mxGraphModel>
```

## References

複雑な図を作成する際は以下のリファレンスを参照すること:

- **`references/xml-reference.md`** — スタイル一覧、エッジルーティング、コンテナ・グループの詳細
- **`references/azure-shapes.md`** — Azure 組み込みシェイプライブラリ（`img/lib/azure2/`）の使い方
- **`references/custom-icons.md`** — 人間・AI・リポジトリ・CI/CD・Wiki などよく使うアクターの組み込みシェイプスタイル集
