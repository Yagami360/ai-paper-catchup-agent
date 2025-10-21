# 🤖 AI Paper Catchup Agent

[![CI](https://github.com/Yagami360/ai-paper-catchup-agent/actions/workflows/ci.yml/badge.svg)](https://github.com/Yagami360/ai-paper-catchup-agent/actions/workflows/ci.yml)
[![Daily Report](https://github.com/Yagami360/ai-paper-catchup-agent/actions/workflows/daily-report.yml/badge.svg)](https://github.com/Yagami360/ai-paper-catchup-agent/actions/workflows/daily-report.yml)
[![Weekly Report](https://github.com/Yagami360/ai-paper-catchup-agent/actions/workflows/weekly-report.yml/badge.svg)](https://github.com/Yagami360/ai-paper-catchup-agent/actions/workflows/weekly-report.yml)
[![Monthly Report](https://github.com/Yagami360/ai-paper-catchup-agent/actions/workflows/monthly-report.yml/badge.svg)](https://github.com/Yagami360/ai-paper-catchup-agent/actions/workflows/monthly-report.yml)
[![Topic Report](https://github.com/Yagami360/ai-paper-catchup-agent/actions/workflows/topic-report.yml/badge.svg)](https://github.com/Yagami360/ai-paper-catchup-agent/actions/workflows/topic-report.yml)
[![Claude](https://github.com/Yagami360/ai-paper-catchup-agent/actions/workflows/claude.yml/badge.svg)](https://github.com/Yagami360/ai-paper-catchup-agent/actions/workflows/claude.yml)
[![Gemini CLI](https://github.com/Yagami360/ai-paper-catchup-agent/actions/workflows/gemini.yml/badge.svg)](https://github.com/Yagami360/ai-paper-catchup-agent/actions/workflows/gemini.yml)

最新AI論文の最新/週次/月次レポート、および特定論文のレポートを GitHub Issue で自動作成する AI Agent です。

- [📅 最新レポート](https://github.com/Yagami360/ai-paper-catchup-agent/issues?q=is%3Aissue%20label%3Areport)
- [📊 週次レポート](https://github.com/Yagami360/ai-paper-catchup-agent/issues?q=is%3Aissue%20label%3Aweekly-report)
- [📈 月次レポート](https://github.com/Yagami360/ai-paper-catchup-agent/issues?q=is%3Aissue%20label%3Amonthly-report)
- [🎯 論文別レポート](https://github.com/Yagami360/ai-paper-catchup-agent/issues?q=is%3Aissue%20label%3Atopic-report)


## 🚀 使用方法

### ☁️ GitHub Actions で動かす場合

1. GitHub secrets and variables を設定する

    - Variables<br>
        - `MODEL_NAME`: 利用するモデル名<br>
            現時点では Claude モデル（`claude-sonnet-4-20250514`, `claude-opus-4-1-20250805` など）と Gemini モデル（`gemini-2.5-flash`, `gemini-2.5-pro` など）をサポートしています
        - `ENABLED_MCP_SERVERS`: 有効にするMCPサーバー（例: `github,huggingface`）<br>
        - `DEEP_RESEARCH`: Deep Research を有効化する場合は `true` を設定（Gemini モデルのみ対応、デフォルト: `false`）<br>

    - Secrets<br>
        - `ANTHROPIC_API_KEY`: Claude モデルを使用する場合<br>
        - `GOOGLE_API_KEY`: Gemini モデルを使用する場合<br>
        - `GEMINI_API_KEY`: Gemini CLI を使用する場合（[Google AI Studio](https://makersuite.google.com/app/apikey) から取得）<br>
        - `HF_TOKEN`: Hugging Face MCPサーバーを使用する場合（[こちら](https://huggingface.co/settings/tokens)から取得）<br>

1. 一定期間間隔でワークフローが自動実行され、GitHub Issue にレポートが自動作成されます

1. （オプション）手動実行したい場合は、以下ワークフローの `Run workflow` から動かすこともできます。

    - [📅 最新レポートのワークフロー](https://github.com/Yagami360/ai-paper-catchup-agent/actions/workflows/daily-report.yml)
    - [📊 週次レポートのワークフロー](https://github.com/Yagami360/ai-paper-catchup-agent/actions/workflows/weekly-report.yml)
    - [📈 月次レポートのワークフロー](https://github.com/Yagami360/ai-paper-catchup-agent/actions/workflows/monthly-report.yml)
    - [🎯 論文別レポートのワークフロー](https://github.com/Yagami360/ai-paper-catchup-agent/actions/workflows/topic-report.yml)
        - `Run workflow` をクリックして、調査したいトピック名を入力してください（例: `AI Agent`, `Vision-Language Models`, `PhysicalAI`）

### 💻 ローカル環境で動かす場合

#### 1️⃣ 依存関係のインストール

```bash
# uvのインストール（まだの場合）
curl -LsSf https://astral.sh/uv/install.sh | sh

# PATHの設定
source $HOME/.local/bin/env

# 依存関係のインストール
make install
```

#### 2️⃣ 環境設定

```bash
# 環境設定ファイルの作成
make setup

# .envファイルを編集してAPIキーを設定
# ANTHROPIC_API_KEY=your_anthropic_api_key_here
# GOOGLE_API_KEY=your_google_api_key_here
# MODEL_NAME=claude-sonnet-4-20250514
# GITHUB_TOKEN=your_github_token_here
# GITHUB_REPOSITORY=your_username/your_repo
# ENABLED_MCP_SERVERS=github,huggingface
# HF_TOKEN=your_huggingface_token_here
# MAX_TOKENS=10000
# NEWS_COUNT=20
# DEEP_RESEARCH=false  # Deep Research を有効化する場合は true（Gemini モデルのみ対応）
```

#### 3️⃣ 実行

```bash
# 📰 最新レポート作成
make run

# 📊 週次レポート作成
make run-weekly

# 📈 月次レポート作成
make run-monthly

# 🎯 論文別レポート作成
make run-topic TOPIC="AI Agent"
```

### 🤖 レポート内容の質疑応答する

作成された Issue レポートの内容について、AI モデルと質疑応答することもできます。

#### Claude と質疑応答する

Issue や PR のコメントで `@claude` とメンションすると、Claude が自動的に日本語で応答します。

**使用例：**
```
@claude この中で最も重要なニュースは何ですか?
@claude OpenAI の最新情報について詳しく教えてください
@claude このレポートの要点を3つにまとめてください
```

#### Gemini と質疑応答する

Issue や PR のコメントで `@gemini-cli` とメンションすると、Gemini が自動的に日本語で応答します。

**使用例：**
```
@gemini-cli Multi-Agent System の実装例を教えてください
@gemini-cli この技術のユースケースは何ですか?
@gemini-cli 今後のトレンドについて教えてください
```

> **Note**: 
> - Claude は `@claude` メンション、Gemini は `@gemini-cli` メンションで呼び出します
> - どちらも Issue コメントおよび PR コメントで利用可能です
> - レポート Issue の内容を理解した上で回答します

## 👨‍💻 開発者向け情報

### 📋 利用可能コマンド

| コマンド | 説明 |
|---------|------|
| `make install` | 📦 依存関係をインストール |
| `make setup` | ⚙️ 開発環境のセットアップ |
| `make run` | 📰 最新レポート作成 |
| `make run-weekly` | 📊 週次レポート生成 |
| `make run-monthly` | 📈 月次レポート生成 |
| `make run-topic TOPIC="トピック名"` | 🎯 トピック別レポート生成 |
| `make test` | 🧪 テストを実行 |
| `make lint` | 🔍 コードのリンティング |
| `make format` | ✨ コードのフォーマット |

### 🔌 MCP サーバー統合

AI Paper Catchup Agent は MCP (Model Context Protocol) サーバーをサポートしており、Claude モデルを使用時に外部ツールやサービスと連携できます。

#### 利用可能な MCP サーバー

1. **GitHub MCP Server** ✅
   - GitHubリポジトリ、Issue、PRへの直接アクセス
   - GitHub Trending からトレンドプロジェクトの取得
   - Code Security アラートの確認

2. **Hugging Face MCP Server** ✅
   - 最新AIモデルの検索
   - データセット、Space、論文の検索
   - モデルの人気度・ダウンロード数の確認

#### MCPサーバーの有効化

**環境変数で設定**:
```bash
# .env ファイルに追加
ENABLED_MCP_SERVERS=github,huggingface
```

**CLIで指定**:
```bash
# GitHub MCP サーバーを使用
uv run python -m src.main --mcp-servers github

# 複数のMCPサーバーを使用
uv run python -m src.main --mcp-servers github,huggingface
```

#### 事前準備

**GitHub MCP Server**:
1. GitHub CLI のインストール:
   ```bash
   # macOS
   brew install gh
   
   # Linux
   sudo apt install gh
   ```

2. 認証:
   ```bash
   gh auth login
   # または
   export GITHUB_TOKEN="your_github_token"
   ```

**Hugging Face MCP Server**:
1. Hugging Face トークンの取得:
   - [Hugging Face Settings](https://huggingface.co/settings/tokens) からアクセストークンを作成
   - トークンタイプは `read` で十分です

2. 環境変数の設定:
   ```bash
   export HF_TOKEN="your_huggingface_token"
   ```

   または `.env` ファイルに追加:
   ```
   HF_TOKEN=your_huggingface_token
   ```

> **Note**: ローカル環境では初回実行時に自動的にログインプロンプトが表示されますが、CI/CD環境では`HF_TOKEN`の設定が必須です

詳細は [`mcp/mcp_servers.yaml`](mcp/mcp_servers.yaml) を参照してください。

