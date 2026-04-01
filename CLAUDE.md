# Presentation Directory

## PowerPoint 作成環境

### Python 環境

標準の `python` / `python3` コマンドは Windows Store スタブのため使用不可。
必ず以下の venv を使用すること。

```bash
PYTHON=".venv/Scripts/python.exe"
```

必要パッケージは `.venv` にインストール済み: `python-pptx`, `Pillow`, `markitdown[pptx]`

### 重要: エンコーディング

Windows 日本語環境では Python のデフォルトエンコーディングが cp932 になるため、
**必ず `PYTHONUTF8=1` を付けて実行すること**。

```bash
PYTHONUTF8=1 .venv/Scripts/python.exe <script> ...
```

付けない場合、pack.py の XSD バリデーションが `cp932 codec can't decode` で失敗する。

---

## テンプレート

会社テンプレート: `format.pptx`（A4サイズ: 9906000 × 6858000 EMU）

### スライドレイアウト一覧

| Layout | 用途 | 主要プレースホルダー |
|--------|------|---------------------|
| slideLayout1 | タイトル（表紙・セクション区切り） | `ctrTitle`, `subTitle` |
| slideLayout2 | 目次 | `title`, `idx=1`（項目リスト）, `idx=10`（ページ番号列） |
| slideLayout3 | コンテンツ（title + body） | `title`, `idx=1` |
| slideLayout4 | コンテンツ（title + body） | `title`, `idx=1` |
| slideLayout5 | はじめに・概要（大テキスト） | `idx=1`（本文のみ、titleなし） |

---

## pptx スキルのワークフロー

スクリプトは全て `/c/Users/ta-mori/.claude/plugins/marketplaces/anthropic-agent-skills/skills/pptx/scripts/` にある。

```bash
PYTHON=".venv/Scripts/python.exe"
SCRIPTS="/c/Users/ta-mori/.claude/plugins/marketplaces/anthropic-agent-skills/skills/pptx/scripts"

# 1. テンプレートを展開
PYTHONUTF8=1 $PYTHON $SCRIPTS/office/unpack.py format.pptx <name>_unpacked/

# 2. スライドを追加（既存スライドを複製）
PYTHONUTF8=1 $PYTHON $SCRIPTS/add_slide.py <name>_unpacked/ slide4.xml
# → 出力された <p:sldId ...> を ppt/presentation.xml の <p:sldIdLst> に追加

# 3. 各スライドの XML を編集
# Edit tool で <name>_unpacked/ppt/slides/slideN.xml を直接編集

# 4. クリーンアップ
PYTHONUTF8=1 $PYTHON $SCRIPTS/clean.py <name>_unpacked/

# 5. パック
PYTHONUTF8=1 $PYTHON $SCRIPTS/office/pack.py <name>_unpacked/ output.pptx --original format.pptx

# テキスト確認
PYTHONUTF8=1 $PYTHON -m markitdown output.pptx
```

### 注意: thumbnail.py は Windows 非対応

`thumbnail.py` は LibreOffice + `socket.AF_UNIX` に依存しており Windows では動作しない。
ビジュアル確認は PowerPoint で直接開いて行うこと。

---

## スライド XML の基本構造

### コンテンツスライド（slideLayout3/4）

```xml
<p:sld>
  <p:cSld><p:spTree>
    <!-- タイトル -->
    <p:sp>
      <p:nvSpPr><p:nvPr><p:ph type="title"/></p:nvPr></p:nvSpPr>
      <p:txBody><a:bodyPr/><a:lstStyle/>
        <a:p><a:r><a:rPr lang="ja-JP" sz="2400" b="1"/><a:t>スライドタイトル</a:t></a:r></a:p>
      </p:txBody>
    </p:sp>
    <!-- 本文 -->
    <p:sp>
      <p:nvSpPr><p:nvPr><p:ph idx="1"/></p:nvPr></p:nvSpPr>
      <p:txBody><a:bodyPr><a:normAutofit/></a:bodyPr><a:lstStyle/>
        <a:p><a:r><a:rPr lang="ja-JP" sz="1600"/><a:t>本文テキスト</a:t></a:r></a:p>
      </p:txBody>
    </p:sp>
  </p:spTree></p:cSld>
  <p:clrMapOvr><a:masterClrMapping/></p:clrMapOvr>
</p:sld>
```

### 表紙スライド（slideLayout1）

```xml
<p:sp>
  <p:nvSpPr><p:nvPr><p:ph type="ctrTitle"/></p:nvPr></p:nvSpPr>
  <p:txBody>...<a:t>メインタイトル</a:t>...</p:txBody>
</p:sp>
<p:sp>
  <p:nvSpPr><p:nvPr><p:ph type="subTitle" idx="1"/></p:nvPr></p:nvSpPr>
  <p:txBody>...<a:t>サブタイトル / 発表者名</a:t>...</p:txBody>
</p:sp>
```
