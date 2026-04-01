# draw.io スキル

## 概要

draw.io 図を作成するためのスキル。カスタムアイコンを使用してアーキテクチャ図やフロー図を作成する。

- `.drawio` ファイルは XML ベースのフォーマット
- カスタム SVG アイコンを `shape=image` スタイルで埋め込み可能
- 出力した `.drawio` ファイルは draw.io（diagrams.net）で開いて編集・エクスポートできる

---

## アイコンライブラリ

アイコンファイルは `skills/drawio/icons/` に格納。

### Azure リソース

| アイコン | ファイル | 用途 |
|----------|----------|------|
| Azure | `azure.svg` | Azure 全般 |
| Blob Storage | `azure-blob-storage.svg` | Azure Blob Storage（画像ストレージ等） |
| App Service | `azure-app-service.svg` | Azure App Service |
| Functions | `azure-functions.svg` | Azure Functions（サーバーレス） |
| SQL Database | `azure-sql-database.svg` | Azure SQL Database |

### 開発ツール

| アイコン | ファイル | 用途 |
|----------|----------|------|
| GitHub | `github.svg` | GitHub リポジトリ / Git |
| GitHub Actions | `github-actions.svg` | GitHub Actions（CI/CD） |
| Growi | `growi.svg` | Growi Wiki |

### 人物・AI

| アイコン | ファイル | 用途 |
|----------|----------|------|
| 人間 | `human.svg` | 人間（Judgment を担う側） |
| AI エージェント | `ai-agent.svg` | AI エージェント（Reckoning を担う側） |

---

## draw.io XML の基本構造

```xml
<mxfile host="app.diagrams.net" agent="Claude">
  <diagram name="Page-1" id="page1">
    <mxGraphModel dx="1422" dy="762" grid="1" gridSize="10" guides="1"
                  tooltips="1" connect="1" arrows="1" fold="1" page="1"
                  pageScale="1" pageWidth="1169" pageHeight="827" math="0" shadow="0">
      <root>
        <!-- 必須: ルート要素 -->
        <mxCell id="0" />
        <mxCell id="1" parent="0" />

        <!-- ここにセル（図形・矢印・アイコン）を配置 -->

      </root>
    </mxGraphModel>
  </diagram>
</mxfile>
```

### セルの種類

#### テキスト付き矩形

```xml
<mxCell id="2" value="ラベルテキスト"
        style="rounded=1;whiteSpace=wrap;html=1;fillColor=#dae8fc;strokeColor=#6c8ebf;"
        vertex="1" parent="1">
  <mxGeometry x="100" y="100" width="160" height="60" as="geometry" />
</mxCell>
```

#### 矢印（接続線）

```xml
<mxCell id="3" value="接続ラベル"
        style="edgeStyle=orthogonalEdgeStyle;rounded=0;orthogonalLoop=1;"
        edge="1" source="2" target="4" parent="1">
  <mxGeometry relative="1" as="geometry" />
</mxCell>
```

#### カスタムアイコン（SVG 画像）

```xml
<mxCell id="5" value="Azure Blob Storage"
        style="shape=image;verticalLabelPosition=bottom;labelBackgroundColor=default;
               verticalAlign=top;aspect=fixed;imageAspect=0;
               image=data:image/svg+xml,BASE64_ENCODED_SVG;"
        vertex="1" parent="1">
  <mxGeometry x="200" y="100" width="48" height="48" as="geometry" />
</mxCell>
```

---

## アイコンの埋め込み方法

### SVG → Base64 変換

```bash
cat skills/drawio/icons/azure-blob-storage.svg | python3 -c "import sys, base64; print(base64.b64encode(sys.stdin.buffer.read()).decode())"
```

### style 属性でのアイコン指定

```
style="shape=image;verticalLabelPosition=bottom;labelBackgroundColor=default;verticalAlign=top;aspect=fixed;imageAspect=0;image=data:image/svg+xml,{BASE64};"
```

### 各アイコンの Base64 エンコード済みデータ

#### azure.svg

```
PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHZpZXdCb3g9IjAgMCA0OCA0OCIgd2lkdGg9IjQ4IiBoZWlnaHQ9IjQ4Ij4KICA8IS0tIEF6dXJlIGxvZ286IHNpbXBsaWZpZWQgZGlhbW9uZCBzaGFwZSAtLT4KICA8ZGVmcz4KICAgIDxsaW5lYXJHcmFkaWVudCBpZD0iYXp1cmVHcmFkIiB4MT0iMCUiIHkxPSIwJSIgeDI9IjEwMCUiIHkyPSIxMDAlIj4KICAgICAgPHN0b3Agb2Zmc2V0PSIwJSIgc3R5bGU9InN0b3AtY29sb3I6IzAwNzhENCIvPgogICAgICA8c3RvcCBvZmZzZXQ9IjEwMCUiIHN0eWxlPSJzdG9wLWNvbG9yOiMwMDVBOUUiLz4KICAgIDwvbGluZWFyR3JhZGllbnQ+CiAgPC9kZWZzPgogIDxwYXRoIGQ9Ik0xMyA2IEwyOCA2IEwzNSAyNCBMMjIgNDIgTDcgNDIgTDIwIDI0IFoiIGZpbGw9InVybCgjYXp1cmVHcmFkKSIvPgogIDxwYXRoIGQ9Ik0yNSAxNCBMNDEgNDIgTDMwIDQyIEwzNSAyNCBaIiBmaWxsPSIjNTBFNkZGIiBvcGFjaXR5PSIwLjgiLz4KPC9zdmc+Cg==
```

#### azure-blob-storage.svg

```
PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHZpZXdCb3g9IjAgMCA0OCA0OCIgd2lkdGg9IjQ4IiBoZWlnaHQ9IjQ4Ij4KICA8IS0tIEF6dXJlIEJsb2IgU3RvcmFnZTogc3RvcmFnZSBjb250YWluZXIgd2l0aCBkYXRhIGJsb2NrcyAtLT4KICA8cmVjdCB4PSI4IiB5PSI2IiB3aWR0aD0iMzIiIGhlaWdodD0iMzYiIHJ4PSIzIiByeT0iMyIgZmlsbD0iIzAwNzhENCIvPgogIDxyZWN0IHg9IjEyIiB5PSIxMSIgd2lkdGg9IjEwIiBoZWlnaHQ9IjciIHJ4PSIxIiBmaWxsPSIjNTBFNkZGIi8+CiAgPHJlY3QgeD0iMjYiIHk9IjExIiB3aWR0aD0iMTAiIGhlaWdodD0iNyIgcng9IjEiIGZpbGw9IiM1MEU2RkYiLz4KICA8cmVjdCB4PSIxMiIgeT0iMjIiIHdpZHRoPSIxMCIgaGVpZ2h0PSI3IiByeD0iMSIgZmlsbD0iIzUwRTZGRiIvPgogIDxyZWN0IHg9IjI2IiB5PSIyMiIgd2lkdGg9IjEwIiBoZWlnaHQ9IjciIHJ4PSIxIiBmaWxsPSIjNTBFNkZGIi8+CiAgPHJlY3QgeD0iMTIiIHk9IjMzIiB3aWR0aD0iMTAiIGhlaWdodD0iNSIgcng9IjEiIGZpbGw9IiM1MEU2RkYiIG9wYWNpdHk9IjAuNiIvPgo8L3N2Zz4K
```

#### azure-app-service.svg

```
PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHZpZXdCb3g9IjAgMCA0OCA0OCIgd2lkdGg9IjQ4IiBoZWlnaHQ9IjQ4Ij4KICA8IS0tIEF6dXJlIEFwcCBTZXJ2aWNlOiBjbG91ZCB3aXRoIGdlYXIgLS0+CiAgPHBhdGggZD0iTTEyIDMyIEM1IDMyIDIgMjYgNiAyMSBDNCAxNCAxMCA5IDE3IDEwIEMxOSA1IDI3IDMgMzMgNyBDMzkgNCA0NiA5IDQ0IDE3IEM0OCAyMSA0NiAyOCA0MCAzMCBDNDIgMzQgMzggMzYgMzMgMzYgTDE1IDM2IEMxMyAzNiAxMiAzNCAxMiAzMiBaIiBmaWxsPSIjMDA3OEQ0Ii8+CiAgPGNpcmNsZSBjeD0iMjQiIGN5PSIyMiIgcj0iNyIgZmlsbD0ibm9uZSIgc3Ryb2tlPSIjZmZmIiBzdHJva2Utd2lkdGg9IjIiLz4KICA8Y2lyY2xlIGN4PSIyNCIgY3k9IjIyIiByPSIyIiBmaWxsPSIjZmZmIi8+CiAgPGxpbmUgeDE9IjI0IiB5MT0iMTUiIHgyPSIyNCIgeTI9IjE3IiBzdHJva2U9IiNmZmYiIHN0cm9rZS13aWR0aD0iMiIvPgogIDxsaW5lIHgxPSIyNCIgeTE9IjI3IiB4Mj0iMjQiIHkyPSIyOSIgc3Ryb2tlPSIjZmZmIiBzdHJva2Utd2lkdGg9IjIiLz4KICA8bGluZSB4MT0iMTciIHkxPSIyMiIgeDI9IjE5IiB5Mj0iMjIiIHN0cm9rZT0iI2ZmZiIgc3Ryb2tlLXdpZHRoPSIyIi8+CiAgPGxpbmUgeDE9IjI5IiB5MT0iMjIiIHgyPSIzMSIgeTI9IjIyIiBzdHJva2U9IiNmZmYiIHN0cm9rZS13aWR0aD0iMiIvPgo8L3N2Zz4K
```

#### azure-functions.svg

```
PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHZpZXdCb3g9IjAgMCA0OCA0OCIgd2lkdGg9IjQ4IiBoZWlnaHQ9IjQ4Ij4KICA8IS0tIEF6dXJlIEZ1bmN0aW9uczogbGlnaHRuaW5nIGJvbHQgaW4gaGV4YWdvbiAtLT4KICA8cG9seWdvbiBwb2ludHM9IjI0LDIgNDIsMTMgNDIsMzUgMjQsNDYgNiwzNSA2LDEzIiBmaWxsPSIjMDA3OEQ0Ii8+CiAgPHBhdGggZD0iTTI3IDEwIEwxOSAyNSBMMjYgMjUgTDIxIDM4IEwzMyAyMSBMMjUgMjEgWiIgZmlsbD0iI0ZGRDcwMCIvPgo8L3N2Zz4K
```

#### azure-sql-database.svg

```
PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHZpZXdCb3g9IjAgMCA0OCA0OCIgd2lkdGg9IjQ4IiBoZWlnaHQ9IjQ4Ij4KICA8IS0tIEF6dXJlIFNRTCBEYXRhYmFzZTogY3lsaW5kZXIgd2l0aCBTUUwgbGFiZWwgLS0+CiAgPGVsbGlwc2UgY3g9IjI0IiBjeT0iMTIiIHJ4PSIxNiIgcnk9IjYiIGZpbGw9IiM1MEU2RkYiLz4KICA8cmVjdCB4PSI4IiB5PSIxMiIgd2lkdGg9IjMyIiBoZWlnaHQ9IjI0IiBmaWxsPSIjMDA3OEQ0Ii8+CiAgPGVsbGlwc2UgY3g9IjI0IiBjeT0iMzYiIHJ4PSIxNiIgcnk9IjYiIGZpbGw9IiMwMDVBOUUiLz4KICA8ZWxsaXBzZSBjeD0iMjQiIGN5PSIxMiIgcng9IjE2IiByeT0iNiIgZmlsbD0iIzUwRTZGRiIvPgogIDx0ZXh0IHg9IjI0IiB5PSIyOCIgdGV4dC1hbmNob3I9Im1pZGRsZSIgZm9udC1mYW1pbHk9IkFyaWFsLCBzYW5zLXNlcmlmIiBmb250LXNpemU9IjEwIiBmb250LXdlaWdodD0iYm9sZCIgZmlsbD0iI2ZmZiI+U1FMPC90ZXh0Pgo8L3N2Zz4K
```

#### github.svg

```
PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHZpZXdCb3g9IjAgMCA0OCA0OCIgd2lkdGg9IjQ4IiBoZWlnaHQ9IjQ4Ij4KICA8IS0tIEdpdCByZXBvc2l0b3J5OiBicmFuY2gvbWVyZ2UgaWNvbiAtLT4KICA8Y2lyY2xlIGN4PSIyNCIgY3k9IjI0IiByPSIyMiIgZmlsbD0iIzI0MjkyRiIvPgogIDxjaXJjbGUgY3g9IjE4IiBjeT0iMTQiIHI9IjMiIGZpbGw9Im5vbmUiIHN0cm9rZT0iI2ZmZiIgc3Ryb2tlLXdpZHRoPSIyIi8+CiAgPGNpcmNsZSBjeD0iMzAiIGN5PSIxNCIgcj0iMyIgZmlsbD0ibm9uZSIgc3Ryb2tlPSIjZmZmIiBzdHJva2Utd2lkdGg9IjIiLz4KICA8Y2lyY2xlIGN4PSIyNCIgY3k9IjM2IiByPSIzIiBmaWxsPSJub25lIiBzdHJva2U9IiNmZmYiIHN0cm9rZS13aWR0aD0iMiIvPgogIDxsaW5lIHgxPSIxOCIgeTE9IjE3IiB4Mj0iMTgiIHkyPSIyNyIgc3Ryb2tlPSIjZmZmIiBzdHJva2Utd2lkdGg9IjIiLz4KICA8bGluZSB4MT0iMzAiIHkxPSIxNyIgeDI9IjMwIiB5Mj0iMjIiIHN0cm9rZT0iI2ZmZiIgc3Ryb2tlLXdpZHRoPSIyIi8+CiAgPHBhdGggZD0iTTE4IDI3IFExOCAzMyAyNCAzMyIgZmlsbD0ibm9uZSIgc3Ryb2tlPSIjZmZmIiBzdHJva2Utd2lkdGg9IjIiLz4KICA8cGF0aCBkPSJNMzAgMjIgUTMwIDI4IDI0IDMzIiBmaWxsPSJub25lIiBzdHJva2U9IiNmZmYiIHN0cm9rZS13aWR0aD0iMiIvPgo8L3N2Zz4K
```

#### github-actions.svg

```
PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHZpZXdCb3g9IjAgMCA0OCA0OCIgd2lkdGg9IjQ4IiBoZWlnaHQ9IjQ4Ij4KICA8IS0tIENJL0NEIFdvcmtmbG93OiBnZWFyIHdpdGggY2lyY3VsYXIgYXJyb3dzIC0tPgogIDxjaXJjbGUgY3g9IjI0IiBjeT0iMjQiIHI9IjIyIiBmaWxsPSIjMjA4OEZGIi8+CiAgPCEtLSBPdXRlciBjaXJjdWxhciBhcnJvdyAtLT4KICA8cGF0aCBkPSJNMjQgOCBBMTYgMTYgMCAxIDEgMTAgMTgiIGZpbGw9Im5vbmUiIHN0cm9rZT0iI2ZmZiIgc3Ryb2tlLXdpZHRoPSIyLjUiIHN0cm9rZS1saW5lY2FwPSJyb3VuZCIvPgogIDxwb2x5Z29uIHBvaW50cz0iMTAsMTIgMTAsMjAgMTYsMTYiIGZpbGw9IiNmZmYiLz4KICA8IS0tIENlbnRlciBnZWFyIC0tPgogIDxjaXJjbGUgY3g9IjI0IiBjeT0iMjQiIHI9IjYiIGZpbGw9Im5vbmUiIHN0cm9rZT0iI2ZmZiIgc3Ryb2tlLXdpZHRoPSIyIi8+CiAgPGNpcmNsZSBjeD0iMjQiIGN5PSIyNCIgcj0iMiIgZmlsbD0iI2ZmZiIvPgogIDxsaW5lIHgxPSIyNCIgeTE9IjE2IiB4Mj0iMjQiIHkyPSIxOSIgc3Ryb2tlPSIjZmZmIiBzdHJva2Utd2lkdGg9IjIiIHN0cm9rZS1saW5lY2FwPSJyb3VuZCIvPgogIDxsaW5lIHgxPSIyNCIgeTE9IjI5IiB4Mj0iMjQiIHkyPSIzMiIgc3Ryb2tlPSIjZmZmIiBzdHJva2Utd2lkdGg9IjIiIHN0cm9rZS1saW5lY2FwPSJyb3VuZCIvPgogIDxsaW5lIHgxPSIxNyIgeTE9IjIwIiB4Mj0iMTkuNSIgeTI9IjIxLjUiIHN0cm9rZT0iI2ZmZiIgc3Ryb2tlLXdpZHRoPSIyIiBzdHJva2UtbGluZWNhcD0icm91bmQiLz4KICA8bGluZSB4MT0iMjguNSIgeTE9IjI2LjUiIHgyPSIzMSIgeTI9IjI4IiBzdHJva2U9IiNmZmYiIHN0cm9rZS13aWR0aD0iMiIgc3Ryb2tlLWxpbmVjYXA9InJvdW5kIi8+CiAgPGxpbmUgeDE9IjE3IiB5MT0iMjgiIHgyPSIxOS41IiB5Mj0iMjYuNSIgc3Ryb2tlPSIjZmZmIiBzdHJva2Utd2lkdGg9IjIiIHN0cm9rZS1saW5lY2FwPSJyb3VuZCIvPgogIDxsaW5lIHgxPSIyOC41IiB5MT0iMjEuNSIgeDI9IjMxIiB5Mj0iMjAiIHN0cm9rZT0iI2ZmZiIgc3Ryb2tlLXdpZHRoPSIyIiBzdHJva2UtbGluZWNhcD0icm91bmQiLz4KPC9zdmc+Cg==
```

#### growi.svg

```
PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHZpZXdCb3g9IjAgMCA0OCA0OCIgd2lkdGg9IjQ4IiBoZWlnaHQ9IjQ4Ij4KICA8IS0tIEdyb3dpIFdpa2k6IG9wZW4gYm9vay93aWtpIGljb24gaW4gR3Jvd2kgZ3JlZW4gLS0+CiAgPHJlY3QgeD0iNCIgeT0iOCIgd2lkdGg9IjQwIiBoZWlnaHQ9IjMyIiByeD0iMiIgZmlsbD0iIzFCN0QzQSIvPgogIDwhLS0gQm9vayBzcGluZSAtLT4KICA8bGluZSB4MT0iMjQiIHkxPSI4IiB4Mj0iMjQiIHkyPSI0MCIgc3Ryb2tlPSIjZmZmIiBzdHJva2Utd2lkdGg9IjEuNSIvPgogIDwhLS0gTGVmdCBwYWdlIGxpbmVzIC0tPgogIDxsaW5lIHgxPSI5IiB5MT0iMTUiIHgyPSIyMCIgeTI9IjE1IiBzdHJva2U9IiNmZmYiIHN0cm9rZS13aWR0aD0iMSIgb3BhY2l0eT0iMC44Ii8+CiAgPGxpbmUgeDE9IjkiIHkxPSIxOSIgeDI9IjIwIiB5Mj0iMTkiIHN0cm9rZT0iI2ZmZiIgc3Ryb2tlLXdpZHRoPSIxIiBvcGFjaXR5PSIwLjgiLz4KICA8bGluZSB4MT0iOSIgeTE9IjIzIiB4Mj0iMTgiIHkyPSIyMyIgc3Ryb2tlPSIjZmZmIiBzdHJva2Utd2lkdGg9IjEiIG9wYWNpdHk9IjAuOCIvPgogIDxsaW5lIHgxPSI5IiB5MT0iMjciIHgyPSIyMCIgeTI9IjI3IiBzdHJva2U9IiNmZmYiIHN0cm9rZS13aWR0aD0iMSIgb3BhY2l0eT0iMC44Ii8+CiAgPGxpbmUgeDE9IjkiIHkxPSIzMSIgeDI9IjE2IiB5Mj0iMzEiIHN0cm9rZT0iI2ZmZiIgc3Ryb2tlLXdpZHRoPSIxIiBvcGFjaXR5PSIwLjgiLz4KICA8IS0tIFJpZ2h0IHBhZ2UgbGluZXMgLS0+CiAgPGxpbmUgeDE9IjI4IiB5MT0iMTUiIHgyPSIzOSIgeTI9IjE1IiBzdHJva2U9IiNmZmYiIHN0cm9rZS13aWR0aD0iMSIgb3BhY2l0eT0iMC44Ii8+CiAgPGxpbmUgeDE9IjI4IiB5MT0iMTkiIHgyPSIzOSIgeTI9IjE5IiBzdHJva2U9IiNmZmYiIHN0cm9rZS13aWR0aD0iMSIgb3BhY2l0eT0iMC44Ii8+CiAgPGxpbmUgeDE9IjI4IiB5MT0iMjMiIHgyPSIzNyIgeTI9IjIzIiBzdHJva2U9IiNmZmYiIHN0cm9rZS13aWR0aD0iMSIgb3BhY2l0eT0iMC44Ii8+CiAgPGxpbmUgeDE9IjI4IiB5MT0iMjciIHgyPSIzOSIgeTI9IjI3IiBzdHJva2U9IiNmZmYiIHN0cm9rZS13aWR0aD0iMSIgb3BhY2l0eT0iMC44Ii8+CiAgPGxpbmUgeDE9IjI4IiB5MT0iMzEiIHgyPSIzNSIgeTI9IjMxIiBzdHJva2U9IiNmZmYiIHN0cm9rZS13aWR0aD0iMSIgb3BhY2l0eT0iMC44Ii8+CiAgPCEtLSBHIGxhYmVsIC0tPgogIDx0ZXh0IHg9IjI0IiB5PSI0NiIgdGV4dC1hbmNob3I9Im1pZGRsZSIgZm9udC1mYW1pbHk9IkFyaWFsLCBzYW5zLXNlcmlmIiBmb250LXNpemU9IjUiIGZpbGw9IiMxQjdEM0EiIGZvbnQtd2VpZ2h0PSJib2xkIj5HUk9XSTwvdGV4dD4KPC9zdmc+Cg==
```

#### human.svg

```
PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHZpZXdCb3g9IjAgMCA0OCA0OCIgd2lkdGg9IjQ4IiBoZWlnaHQ9IjQ4Ij4KICA8IS0tIEh1bWFuOiBwZXJzb24gc2lsaG91ZXR0ZSBpY29uIC0tPgogIDwhLS0gSGVhZCAtLT4KICA8Y2lyY2xlIGN4PSIyNCIgY3k9IjEyIiByPSI3IiBmaWxsPSIjNEE1NTY4Ii8+CiAgPCEtLSBCb2R5IC0tPgogIDxwYXRoIGQ9Ik0xMiA0NCBMMTIgMzIgQzEyIDI1IDE3IDIwIDI0IDIwIEMzMSAyMCAzNiAyNSAzNiAzMiBMMzYgNDQgWiIgZmlsbD0iIzRBNTU2OCIvPgogIDwhLS0gU2hvdWxkZXIgbGluZSBmb3IgZGVwdGggLS0+CiAgPHBhdGggZD0iTTggMzYgQzggMjcgMTUgMjAgMjQgMjAgQzMzIDIwIDQwIDI3IDQwIDM2IiBmaWxsPSJub25lIiBzdHJva2U9IiM0QTU1NjgiIHN0cm9rZS13aWR0aD0iMSIvPgo8L3N2Zz4K
```

#### ai-agent.svg

```
PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHZpZXdCb3g9IjAgMCA0OCA0OCIgd2lkdGg9IjQ4IiBoZWlnaHQ9IjQ4Ij4KICA8IS0tIEFJIEFnZW50OiByb2JvdC9jaXJjdWl0IGJyYWluIGljb24gLS0+CiAgPCEtLSBIZWFkIG91dGxpbmUgLS0+CiAgPHJlY3QgeD0iMTAiIHk9IjgiIHdpZHRoPSIyOCIgaGVpZ2h0PSIyNCIgcng9IjQiIGZpbGw9IiM3QzNBRUQiLz4KICA8IS0tIEV5ZXMgLS0+CiAgPGNpcmNsZSBjeD0iMTgiIGN5PSIxOCIgcj0iMyIgZmlsbD0iI2ZmZiIvPgogIDxjaXJjbGUgY3g9IjMwIiBjeT0iMTgiIHI9IjMiIGZpbGw9IiNmZmYiLz4KICA8Y2lyY2xlIGN4PSIxOCIgY3k9IjE4IiByPSIxLjUiIGZpbGw9IiM3QzNBRUQiLz4KICA8Y2lyY2xlIGN4PSIzMCIgY3k9IjE4IiByPSIxLjUiIGZpbGw9IiM3QzNBRUQiLz4KICA8IS0tIE1vdXRoL3NwZWFrZXIgLS0+CiAgPHJlY3QgeD0iMTYiIHk9IjI1IiB3aWR0aD0iMTYiIGhlaWdodD0iMyIgcng9IjEiIGZpbGw9IiNmZmYiIG9wYWNpdHk9IjAuOCIvPgogIDwhLS0gQW50ZW5uYSAtLT4KICA8bGluZSB4MT0iMjQiIHkxPSI4IiB4Mj0iMjQiIHkyPSIzIiBzdHJva2U9IiM3QzNBRUQiIHN0cm9rZS13aWR0aD0iMiIvPgogIDxjaXJjbGUgY3g9IjI0IiBjeT0iMiIgcj0iMiIgZmlsbD0iI0E3OEJGQSIvPgogIDwhLS0gRWFycy9zZW5zb3JzIC0tPgogIDxyZWN0IHg9IjUiIHk9IjE0IiB3aWR0aD0iNSIgaGVpZ2h0PSI4IiByeD0iMiIgZmlsbD0iI0E3OEJGQSIvPgogIDxyZWN0IHg9IjM4IiB5PSIxNCIgd2lkdGg9IjUiIGhlaWdodD0iOCIgcng9IjIiIGZpbGw9IiNBNzhCRkEiLz4KICA8IS0tIEJvZHkgY29ubmVjdGlvbiAtLT4KICA8cmVjdCB4PSIxOCIgeT0iMzIiIHdpZHRoPSIxMiIgaGVpZ2h0PSI0IiByeD0iMSIgZmlsbD0iIzdDM0FFRCIvPgogIDwhLS0gQ2lyY3VpdCBwYXR0ZXJucyBvbiBib2R5IC0tPgogIDxsaW5lIHgxPSIyMCIgeTE9IjM2IiB4Mj0iMjAiIHkyPSI0MiIgc3Ryb2tlPSIjN0MzQUVEIiBzdHJva2Utd2lkdGg9IjEuNSIvPgogIDxsaW5lIHgxPSIyNCIgeTE9IjM2IiB4Mj0iMjQiIHkyPSI0NCIgc3Ryb2tlPSIjN0MzQUVEIiBzdHJva2Utd2lkdGg9IjEuNSIvPgogIDxsaW5lIHgxPSIyOCIgeTE9IjM2IiB4Mj0iMjgiIHkyPSI0MiIgc3Ryb2tlPSIjN0MzQUVEIiBzdHJva2Utd2lkdGg9IjEuNSIvPgogIDxjaXJjbGUgY3g9IjIwIiBjeT0iNDMiIHI9IjEuNSIgZmlsbD0iI0E3OEJGQSIvPgogIDxjaXJjbGUgY3g9IjI0IiBjeT0iNDUiIHI9IjEuNSIgZmlsbD0iI0E3OEJGQSIvPgogIDxjaXJjbGUgY3g9IjI4IiBjeT0iNDMiIHI9IjEuNSIgZmlsbD0iI0E3OEJGQSIvPgo8L3N2Zz4K
```

---

## テンプレート

### アーキテクチャ図テンプレート（最小構成）

```xml
<mxfile host="app.diagrams.net" agent="Claude">
  <diagram name="Architecture" id="arch1">
    <mxGraphModel dx="1422" dy="762" grid="1" gridSize="10" guides="1"
                  tooltips="1" connect="1" arrows="1" fold="1" page="1"
                  pageScale="1" pageWidth="1169" pageHeight="827" math="0" shadow="0">
      <root>
        <mxCell id="0" />
        <mxCell id="1" parent="0" />

        <!-- アイコン例: style の image= に上記 Base64 を入れる -->
        <mxCell id="icon1" value="Azure Blob Storage"
                style="shape=image;verticalLabelPosition=bottom;labelBackgroundColor=default;verticalAlign=top;aspect=fixed;imageAspect=0;image=data:image/svg+xml,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHZpZXdCb3g9IjAgMCA0OCA0OCIgd2lkdGg9IjQ4IiBoZWlnaHQ9IjQ4Ij4KICA8IS0tIEF6dXJlIEJsb2IgU3RvcmFnZTogc3RvcmFnZSBjb250YWluZXIgd2l0aCBkYXRhIGJsb2NrcyAtLT4KICA8cmVjdCB4PSI4IiB5PSI2IiB3aWR0aD0iMzIiIGhlaWdodD0iMzYiIHJ4PSIzIiByeT0iMyIgZmlsbD0iIzAwNzhENCIvPgogIDxyZWN0IHg9IjEyIiB5PSIxMSIgd2lkdGg9IjEwIiBoZWlnaHQ9IjciIHJ4PSIxIiBmaWxsPSIjNTBFNkZGIi8+CiAgPHJlY3QgeD0iMjYiIHk9IjExIiB3aWR0aD0iMTAiIGhlaWdodD0iNyIgcng9IjEiIGZpbGw9IiM1MEU2RkYiLz4KICA8cmVjdCB4PSIxMiIgeT0iMjIiIHdpZHRoPSIxMCIgaGVpZ2h0PSI3IiByeD0iMSIgZmlsbD0iIzUwRTZGRiIvPgogIDxyZWN0IHg9IjI2IiB5PSIyMiIgd2lkdGg9IjEwIiBoZWlnaHQ9IjciIHJ4PSIxIiBmaWxsPSIjNTBFNkZGIi8+CiAgPHJlY3QgeD0iMTIiIHk9IjMzIiB3aWR0aD0iMTAiIGhlaWdodD0iNSIgcng9IjEiIGZpbGw9IiM1MEU2RkYiIG9wYWNpdHk9IjAuNiIvPgo8L3N2Zz4K;"
                vertex="1" parent="1">
          <mxGeometry x="100" y="100" width="48" height="48" as="geometry" />
        </mxCell>

        <!-- グループ化する矩形 -->
        <mxCell id="group1" value="Azure クラウド"
                style="rounded=1;whiteSpace=wrap;html=1;fillColor=#E6F0FF;strokeColor=#0078D4;dashed=1;verticalAlign=top;fontStyle=1;fontSize=14;"
                vertex="1" parent="1">
          <mxGeometry x="50" y="50" width="300" height="200" as="geometry" />
        </mxCell>

        <!-- 矢印 -->
        <mxCell id="edge1" value="同期"
                style="edgeStyle=orthogonalEdgeStyle;rounded=0;orthogonalLoop=1;jetSize=auto;html=1;"
                edge="1" source="icon1" target="icon2" parent="1">
          <mxGeometry relative="1" as="geometry" />
        </mxCell>
      </root>
    </mxGraphModel>
  </diagram>
</mxfile>
```

### フロー図テンプレート（判断 → 演算ループ）

```xml
<mxfile host="app.diagrams.net" agent="Claude">
  <diagram name="Flow" id="flow1">
    <mxGraphModel dx="1422" dy="762" grid="1" gridSize="10" guides="1"
                  tooltips="1" connect="1" arrows="1" fold="1" page="1"
                  pageScale="1" pageWidth="1169" pageHeight="827" math="0" shadow="0">
      <root>
        <mxCell id="0" />
        <mxCell id="1" parent="0" />

        <!-- 人間アイコン -->
        <mxCell id="human1" value="人間（Judgment）"
                style="shape=image;verticalLabelPosition=bottom;labelBackgroundColor=default;verticalAlign=top;aspect=fixed;imageAspect=0;image=data:image/svg+xml,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHZpZXdCb3g9IjAgMCA0OCA0OCIgd2lkdGg9IjQ4IiBoZWlnaHQ9IjQ4Ij4KICA8IS0tIEh1bWFuOiBwZXJzb24gc2lsaG91ZXR0ZSBpY29uIC0tPgogIDwhLS0gSGVhZCAtLT4KICA8Y2lyY2xlIGN4PSIyNCIgY3k9IjEyIiByPSI3IiBmaWxsPSIjNEE1NTY4Ii8+CiAgPCEtLSBCb2R5IC0tPgogIDxwYXRoIGQ9Ik0xMiA0NCBMMTIgMzIgQzEyIDI1IDE3IDIwIDI0IDIwIEMzMSAyMCAzNiAyNSAzNiAzMiBMMzYgNDQgWiIgZmlsbD0iIzRBNTU2OCIvPgogIDwhLS0gU2hvdWxkZXIgbGluZSBmb3IgZGVwdGggLS0+CiAgPHBhdGggZD0iTTggMzYgQzggMjcgMTUgMjAgMjQgMjAgQzMzIDIwIDQwIDI3IDQwIDM2IiBmaWxsPSJub25lIiBzdHJva2U9IiM0QTU1NjgiIHN0cm9rZS13aWR0aD0iMSIvPgo8L3N2Zz4K;"
                vertex="1" parent="1">
          <mxGeometry x="100" y="200" width="48" height="48" as="geometry" />
        </mxCell>

        <!-- AIアイコン -->
        <mxCell id="ai1" value="AI Agent（Reckoning）"
                style="shape=image;verticalLabelPosition=bottom;labelBackgroundColor=default;verticalAlign=top;aspect=fixed;imageAspect=0;image=data:image/svg+xml,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHZpZXdCb3g9IjAgMCA0OCA0OCIgd2lkdGg9IjQ4IiBoZWlnaHQ9IjQ4Ij4KICA8IS0tIEFJIEFnZW50OiByb2JvdC9jaXJjdWl0IGJyYWluIGljb24gLS0+CiAgPCEtLSBIZWFkIG91dGxpbmUgLS0+CiAgPHJlY3QgeD0iMTAiIHk9IjgiIHdpZHRoPSIyOCIgaGVpZ2h0PSIyNCIgcng9IjQiIGZpbGw9IiM3QzNBRUQiLz4KICA8IS0tIEV5ZXMgLS0+CiAgPGNpcmNsZSBjeD0iMTgiIGN5PSIxOCIgcj0iMyIgZmlsbD0iI2ZmZiIvPgogIDxjaXJjbGUgY3g9IjMwIiBjeT0iMTgiIHI9IjMiIGZpbGw9IiNmZmYiLz4KICA8Y2lyY2xlIGN4PSIxOCIgY3k9IjE4IiByPSIxLjUiIGZpbGw9IiM3QzNBRUQiLz4KICA8Y2lyY2xlIGN4PSIzMCIgY3k9IjE4IiByPSIxLjUiIGZpbGw9IiM3QzNBRUQiLz4KICA8IS0tIE1vdXRoL3NwZWFrZXIgLS0+CiAgPHJlY3QgeD0iMTYiIHk9IjI1IiB3aWR0aD0iMTYiIGhlaWdodD0iMyIgcng9IjEiIGZpbGw9IiNmZmYiIG9wYWNpdHk9IjAuOCIvPgogIDwhLS0gQW50ZW5uYSAtLT4KICA8bGluZSB4MT0iMjQiIHkxPSI4IiB4Mj0iMjQiIHkyPSIzIiBzdHJva2U9IiM3QzNBRUQiIHN0cm9rZS13aWR0aD0iMiIvPgogIDxjaXJjbGUgY3g9IjI0IiBjeT0iMiIgcj0iMiIgZmlsbD0iI0E3OEJGQSIvPgogIDwhLS0gRWFycy9zZW5zb3JzIC0tPgogIDxyZWN0IHg9IjUiIHk9IjE0IiB3aWR0aD0iNSIgaGVpZ2h0PSI4IiByeD0iMiIgZmlsbD0iI0E3OEJGQSIvPgogIDxyZWN0IHg9IjM4IiB5PSIxNCIgd2lkdGg9IjUiIGhlaWdodD0iOCIgcng9IjIiIGZpbGw9IiNBNzhCRkEiLz4KICA8IS0tIEJvZHkgY29ubmVjdGlvbiAtLT4KICA8cmVjdCB4PSIxOCIgeT0iMzIiIHdpZHRoPSIxMiIgaGVpZ2h0PSI0IiByeD0iMSIgZmlsbD0iIzdDM0FFRCIvPgogIDwhLS0gQ2lyY3VpdCBwYXR0ZXJucyBvbiBib2R5IC0tPgogIDxsaW5lIHgxPSIyMCIgeTE9IjM2IiB4Mj0iMjAiIHkyPSI0MiIgc3Ryb2tlPSIjN0MzQUVEIiBzdHJva2Utd2lkdGg9IjEuNSIvPgogIDxsaW5lIHgxPSIyNCIgeTE9IjM2IiB4Mj0iMjQiIHkyPSI0NCIgc3Ryb2tlPSIjN0MzQUVEIiBzdHJva2Utd2lkdGg9IjEuNSIvPgogIDxsaW5lIHgxPSIyOCIgeTE9IjM2IiB4Mj0iMjgiIHkyPSI0MiIgc3Ryb2tlPSIjN0MzQUVEIiBzdHJva2Utd2lkdGg9IjEuNSIvPgogIDxjaXJjbGUgY3g9IjIwIiBjeT0iNDMiIHI9IjEuNSIgZmlsbD0iI0E3OEJGQSIvPgogIDxjaXJjbGUgY3g9IjI0IiBjeT0iNDUiIHI9IjEuNSIgZmlsbD0iI0E3OEJGQSIvPgogIDxjaXJjbGUgY3g9IjI4IiBjeT0iNDMiIHI9IjEuNSIgZmlsbD0iI0E3OEJGQSIvPgo8L3N2Zz4K;"
                vertex="1" parent="1">
          <mxGeometry x="400" y="200" width="48" height="48" as="geometry" />
        </mxCell>

        <!-- 判断→演算の矢印 -->
        <mxCell id="edge_j_to_r" value="指示・問いを渡す"
                style="edgeStyle=orthogonalEdgeStyle;rounded=1;orthogonalLoop=1;jetSize=auto;html=1;strokeColor=#4A5568;"
                edge="1" source="human1" target="ai1" parent="1">
          <mxGeometry relative="1" as="geometry" />
        </mxCell>

        <!-- 演算→判断の矢印 -->
        <mxCell id="edge_r_to_j" value="候補・結果を返す"
                style="edgeStyle=orthogonalEdgeStyle;rounded=1;orthogonalLoop=1;jetSize=auto;html=1;strokeColor=#7C3AED;exitX=0.5;exitY=1;exitDx=0;exitDy=0;entryX=0.5;entryY=1;entryDx=0;entryDy=0;"
                edge="1" source="ai1" target="human1" parent="1">
          <mxGeometry relative="1" as="geometry">
            <Array as="points">
              <mxPoint x="424" y="310" />
              <mxPoint x="124" y="310" />
            </Array>
          </mxGeometry>
        </mxCell>
      </root>
    </mxGraphModel>
  </diagram>
</mxfile>
```

---

## ワークフロー

### 1. .drawio ファイルの作成

テンプレートを元に `.drawio` ファイルを作成する。

```bash
# テンプレートからコピー、または直接 XML を作成
cp template.drawio output.drawio
```

### 2. XML の編集

- アイコンの `image=data:image/svg+xml,{BASE64};` に上記のエンコード済みデータを使用
- `<mxGeometry x="" y="" width="" height="">` で位置とサイズを調整
- `value=""` でラベルテキストを設定
- `source=""` `target=""` で矢印の接続先を指定

### 3. 確認とエクスポート

- `.drawio` ファイルを draw.io（https://app.diagrams.net/）で開いて確認
- SVG または PNG にエクスポートして Markdown に埋め込み

### 4. Markdown への埋め込み

```markdown
![図の説明](./diagram.drawio.svg)
```

または draw.io 対応の Markdown ビューアでは `.drawio` を直接参照可能。

---

## スタイルガイド

### 色の対応

| 要素 | 色 | カラーコード |
|------|-----|-------------|
| Azure リソース | Azure Blue | `#0078D4` |
| GitHub | Dark | `#24292F` |
| GitHub Actions | Blue | `#2088FF` |
| Growi | Green | `#1B7D3A` |
| 人間（Judgment） | Gray | `#4A5568` |
| AI（Reckoning） | Purple | `#7C3AED` |
| 判断の領域 | Light Gray 背景 | `#F7FAFC` |
| 演算の領域 | Light Purple 背景 | `#F5F3FF` |

### レイアウトのコツ

- アイコンサイズ: 通常 `48×48`、強調時 `64×64`
- グループ化にはダッシュ付き矩形を使用（`dashed=1`）
- 矢印のラベルは短く（動詞1つ + 名詞1つ程度）
- 判断と演算の領域を色分けして視覚的に区別する
