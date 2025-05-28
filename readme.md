# 特許PDF構造化ライブラリ

特許PDFからマルチモーダル生成AIを使用して構造化されたJSONを抽出するPythonライブラリです。

## 機能

- 特許PDFをマルチモーダル生成AIに直接送信して構造化情報を抽出
- 複数の生成AIプロバイダーをサポート:
  - Google Gemini
  - OpenAI GPT Vision
  - Anthropic Claude
- カスタマイズ可能なJSONスキーマ
- 生成AI設定の調整（temperature、max_tokens）

## インストール

```bash
pip install -r requirements.txt
```

## 使用方法

### ライブラリとして使用

```python
from patent_extractor import PatentExtractor

# 基本的な使用法
extractor = PatentExtractor(
    model_name="gemini-1.5-pro",  # デフォルトモデル
    api_key="YOUR_API_KEY"
)
result = extractor.process_patent_pdf("path/to/patent.pdf")

# 結果を保存
import json
with open("output.json", "w", encoding="utf-8") as f:
    json.dump(result, f, indent=2, ensure_ascii=False)
```

### カスタムJSONスキーマの指定

```python
import json
from patent_extractor import PatentExtractor

# JSONスキーマを読み込み
with open("schema.json", "r", encoding="utf-8") as f:
    schema = json.load(f)

# スキーマを指定してエクストラクタを初期化
extractor = PatentExtractor(
    model_name="gpt-4o",
    api_key="YOUR_API_KEY",
    json_schema=schema
)

# PDFを処理
result = extractor.process_patent_pdf("patent.pdf")
```

### カスタムプロンプトと生成設定の指定

```python
from patent_extractor import PatentExtractor

# カスタムプロンプトと生成設定を指定
extractor = PatentExtractor(
    model_name="claude-3-opus-20240229",
    api_key="YOUR_API_KEY",
    user_prompt="特許文書から次のセクションを重点的に抽出してください：請求項、発明の詳細な説明、図面の簡単な説明",
    temperature=0.2,    # より創造的な出力（デフォルトは0.1）
    max_tokens=8192     # より長い出力を許可（デフォルトは4096）
)

# PDFを処理
result = extractor.process_patent_pdf("patent.pdf")
```

### コマンドラインからの使用

```bash
# 基本的な使用法
python patent_extractor.py path/to/patent.pdf --api-key YOUR_API_KEY

# モデルと生成パラメータを指定
python patent_extractor.py path/to/patent.pdf --model gpt-4o --api-key YOUR_API_KEY --temperature 0.3 --max-tokens 8192

# スキーマとカスタムプロンプトを指定して結果を保存
python patent_extractor.py path/to/patent.pdf --model claude-3-opus-20240229 --api-key YOUR_API_KEY --schema schema.json --prompt "カスタム指示" --output result.json
```

## 環境変数の使用

APIキーは環境変数からも取得できます：

```python
import os
from patent_extractor import PatentExtractor

# 環境変数を設定（必要に応じて）
os.environ["GOOGLE_API_KEY"] = "your-api-key"
# または
os.environ["OPENAI_API_KEY"] = "your-api-key"
# または
os.environ["ANTHROPIC_API_KEY"] = "your-api-key"

# APIキーを明示的に指定せずに初期化
extractor = PatentExtractor(model_name="gemini-1.5-pro")  # 環境変数からAPIキーを取得
```

## サポートされるモデル

### Google Gemini
- gemini-1.5-pro
- gemini-1.5-flash

### OpenAI
- gpt-4o
- gpt-4-vision-preview

### Anthropic Claude
- claude-3-opus-20240229
- claude-3-sonnet-20240229

## 生成パラメータの調整

### temperature
- 範囲: 0.0〜1.0
- 低い値（0.0〜0.3）: より確定的で一貫性のある出力
- 高い値（0.7〜1.0）: より多様でクリエイティブな出力
- デフォルト: 0.1（安定した構造化データに適切）

### max_tokens
- 生成AIが出力できる最大トークン数
- より大きな値: より詳細で長い出力が可能
- より小さな値: 簡潔な出力を強制
- デフォルト: 4096

## 必要条件

- Python 3.8+
- google-generativeai / openai / anthropic パッケージ（APIに応じて）

## ライセンス

MIT
