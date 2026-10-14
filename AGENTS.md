# AGENTS.md

## 共通の承認・安全方針

共通方針の正本は `hashaseg9999-a11y/my-obsidian-notes` の `AGENTS.md` とし、以下をこのリポジトリにも適用する。既存の設計・検証・データ保護の制約は維持する。

### 依頼された変更の完了フロー

- hasさんが対象・目的・範囲を指定して実装や文書変更を依頼した場合、その範囲の実装・関連テストと検証から、トピックブランチ作成・commit・push・PR作成・URL報告までを一括した作業として進める。これらのGit操作だけを理由に途中で追加承認を求めない。監査・調査・提案のみの依頼では編集しない。依頼外の変更には適用しない。
- 実装と関連検証が完了したら、適切なトピックブランチを作成して成果を保存する。default / main / masterを保護するため、必要なら編集開始前にブランチを作成してよい。検証のまとまりごとに小さくcommitし、未コミットのまま不要に中断しない。
- stageするのは今回の変更だけとし、既存の未コミット変更・無関係なファイルを混ぜない。push先は対象リポジトリのトピックブランチに限り、default / main / masterへ直接pushしない。公開範囲を広げたり、別リポジトリやupstreamへ送ったりしない。
- 1PRは原則1テーマとし、同目的のopen PRがあれば重複作成せず利用する。PR本文には What / Why / Impact / Verification を記載し、実際の差分、実行した検証と結果、未実施・失敗・確認不能を区別する。PRのURLと残る阻害要因を報告する。
- 検証不能や失敗を成功扱いにしない。安全に保存できる成果は未完了を明示したDraft PRに残せるが、データ保護違反や承認範囲外の外部影響がある場合はpush・PR作成前に停止して報告する。必須検証や個別の停止条件を省略しない。
- 個人情報・患者情報・医療情報・秘密情報・認証情報を外部送信またはcommitしない。差分だけでなく新たにpushするcommit履歴とPR本文も確認し、検証には架空データを使う。既存のOpaque Fileやより厳しいデータ保護規則を緩めない。
- pushやPR作成が本番反映・公開範囲拡大・追加課金を引き起こす場合は、その操作前にhasさんへ確認する。Gitの標準フローは、無関係な送信・投稿・申請・購入・予定登録・Issue作成・外部タスク作成を許可しない。
- この方針は各ツールの上位安全規則、権限、承認機構を上書きしない。利用できない操作や追加の承認が必要な場合は、完了済みの範囲と具体的な阻害要因を報告する。

### マージ・本番反映・破壊的操作

- PRをdefault / main / masterへマージするには、対象PRについてhasさんの明示承認を必須とする。実装・commit・push・PR作成の依頼や承認をマージ承認に読み替えない。自動マージを実行せず、GitHubのauto-mergeも有効化しない。
- 承認後も、必要なCIの成功、merge conflictがないこと、未解決のレビュー指摘がないこと、依頼どおりの差分と検証結果を確認してからマージする。業務判断や品質評価が必要な変更は、AIだけで合格判定しない。重要な変更（安全・承認境界、認証、本番、保存方式、データ構造、外部連携など）は、実装担当と独立したレビューまたはhasさんによる確認を必要とする。
- 本番環境・外部本番サービスへのdeploy・反映は、対象と影響についてhasさんの明示承認を得る。マージで自動deployされる場合も、マージ承認とは別に本番反映まで承認されていることを確認する。
- `git reset --hard`、force push、履歴書換え、未追跡ファイルの不可逆な一括削除などの破壊的操作は、実行前に対象・影響を示して確認する。個別ルールで禁止される操作は引き続き禁止する。
- 恒久削除、追加課金、認証・権限の変更、公開範囲の拡大は実行前に確認する。リポジトリ固有の公開禁止・自動deploy禁止・レビュー承認必須などの追加制約を維持する。


このリポジトリは [`natural-japanese`](./skills/natural-japanese/SKILL.md) という Agent Skill を配布するためのものです。
スキル本体は `skills/natural-japanese/` にあり、`SKILL.md` と `references/`・`scripts/`・`assets/` で構成されます。

## このスキルについて

仕事の日本語文書を読みやすくわかりやすく書く・直すためのスキルです。議事録・調査レポート・社内ガイド・リサーチメモ・スライド構成といったビジネス文書から、note・ブログ・エッセイまで扱います。AI臭さの除去は、このスキルの一工程として組み込まれています。

設計は二軸です。

- **検出は機械、判断はAI**: 疑いの検出は `skills/natural-japanese/scripts/lint.py`（sudachipy による形態素解析）が決定的に行い、どう直すかはAIが文脈で判断する
- **事後修正より生成時制約**: 書いた後にAI臭を消すより、書く前の設計（読者・主メッセージ・見出しスケルトン）と書くときの制約（`skills/natural-japanese/references/writing-constitution.md` の文体憲法12箇条）で発生自体を防ぐ

文書タイプ別の型は `skills/natural-japanese/references/doctypes/`、詳しい工程は [`SKILL.md`](./skills/natural-japanese/SKILL.md) を参照してください。

## openskills 経由で読み込む場合

```bash
npx openskills install coji/natural-japanese
npx openskills sync
```

`npx openskills sync` がこの AGENTS.md 配下に `<available_skills>` ブロックを生成し、
Claude Code 以外のエージェント（Cursor, ChatGPT, Codex 等）からもこのスキルを利用できるようにします。

## 検査スクリプトの実行

Python の実行は [uv](https://docs.astral.sh/uv/) を前提にしています。`pip install` や仮想環境の手動作成は不要です。機械検査層は役割ごとに3つのエントリに分かれています。

```bash
uv run skills/natural-japanese/scripts/lint.py path/to/draft.md      # 疑いの検出（--json / --genre / --baseline）
uv run skills/natural-japanese/scripts/outline.py path/to/draft.md   # スケルトン抽出（構造レビューの入力）
uv run skills/natural-japanese/scripts/terms.py path/to/draft.md     # 専門用語の初出・説明有無の一覧
```

依存関係（sudachipy, sudachidict-core）は各スクリプト冒頭の PEP 723 インラインメタデータで宣言されているため、
`uv run` が自動的に解決します。共有基盤は `skills/natural-japanese/scripts/textcore.py` にまとまっています。
