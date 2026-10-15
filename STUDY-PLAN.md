# FDE 学習プラン(4週間)

[README-AWS.md](README-AWS.md) のロードマップを、次の前提で圧縮した個人用プランです。

## 前提

- **ゴール:** 1ヶ月以内に FDE / フルサイクルエンジニア職へ転職する。面接は既に進行中。並行して FDE マインドを身につける。
- **時間:** 週 5〜7 時間。業務の合間の「内職」で読み・考える作業を行い、手を動かす作業は夜と週末に回す。
- **スキル(経歴書より):** バックエンド約 5 年(Python / Java)、LangGraph・Google ADK によるエージェント基盤の設計・実装、チームリード、要件定義、障害対応・運用立ち上げの経験がある。クラウドは Azure が中心。
- **ギャップ:** AWS の知識(IAM / VPC / Bedrock / AgentCore)、RAG、LLM 評価(Evals)、データ基盤(S3 / Athena / Iceberg)。
- **方針:** すでに強い Phase 3(コンサル思考)と Applied AI は「面接で語れる形」に整える。ギャップは AWS・RAG・Evals に絞って埋める。エアギャップ、Spark、EKS の詳細は後回しにする。

> **内職の注意:** 学習には私物の端末と個人 AWS アカウントを使い、会社の環境・データ・コードは持ち込まない。
> **公開リポジトリの注意:** 職務経歴書など個人情報はこのリポジトリに置かない(`.info/` は gitignore 済み)。

## 時間配分の目安(週 6 時間)

| 枠 | 時間 | 内容 |
|---|---|---|
| 内職(読む・考える) | 約 3h | 記事、ドキュメント、ケース問題の回答作り、面接ストーリーの整理 |
| 夜・週末(手を動かす) | 約 3h | AWS ハンズオン、アウトプットの作成 |

面接が入った週は、その企業の調査と想定問答を最優先にし、残りの時間でプランを進めます。

---

## Week 1: FDE マインドと「経歴の翻訳」

**目的:** 自分の経験を FDE の言葉(Delta、Day 2、System of Record)で語れるようにする。面接に最も効く週。

- [ ] 内職: [Palantir: Dev vs. Delta](https://blog.palantir.com/dev-versus-delta-demystifying-engineering-roles-at-palantir-ad44c2a6e87) を読む
- [ ] 内職: README-AWS の「FDE Persona & Mission」「Soft Stack」「Discovery Checklist」を通読する
- [ ] 内職: 「C.A.S.E.」フレームと「Hospital Readmission」ケースを読み、自分なら最初の 30 日をどう動くか書く
- [ ] 内職: 高頻度質問 3 問(5PB 移行、敵対的ステークホルダー、<100ms 不正検知)に声に出して答える
- [ ] 内職: 経歴書の実績を 3 本選び、「顧客の課題 → 見つけた Delta → 判断と根拠 → 結果 → Day 2 の設計」の形で書き直す
  - 候補: 要件変更を想定したテーブル駆動のワークフロー設計、ロックイン回避のための抽象化、障害対応の運用改善(クローズまでの日数短縮)
- [ ] 夜: 個人 AWS アカウントを作成し、**最初に予算アラートを設定**する。ルートユーザーに MFA を設定し、IAM Identity Center で日常用ユーザーを作る
- [ ] 夜: Bedrock でモデルを呼び出せるようにする(リージョンとモデルアクセスを確認)

**成果物:** 面接ストーリー 3 本のメモ、ケース問題の回答メモ 1 本、AWS アカウント

## Week 2: AWS の土台(Azure からの翻訳)

**目的:** 「Azure なら分かる」を「AWS ではこう」と言い換えられる状態にする。

- [ ] 内職: Azure ↔ AWS 対応表を自分で作る(下の表を雛形にする)
- [ ] 内職: [AWS Well-Architected Framework](https://docs.aws.amazon.com/wellarchitected/latest/framework/welcome.html) の 6 本柱を読む
- [ ] 内職: README-AWS の Phase 2(アカウント分離、リージョナル VPC、SCP/RCP、Data Perimeter)を、顧客に説明する想定で要約する
- [ ] 夜: S3 に CSV/Parquet を置き、Athena でクエリする。同じデータを DuckDB でローカルでも分析して比較する
- [ ] 夜: S3 / Bedrock への VPC endpoint(PrivateLink)を 1 つ作る。経験済みの Azure Private Link と対比してメモする
- [ ] 夜(余裕があれば): 上記の構成を Terraform か CDK で再現する

**成果物:** Azure ↔ AWS 対応表、Athena クエリの手順メモ

| Azure(経験あり) | AWS |
|---|---|
| Subscription / Management Group | Account / AWS Organizations |
| Entra ID | IAM Identity Center |
| App Service | Lambda / ECS on Fargate / App Runner |
| Azure OpenAI | Amazon Bedrock |
| Private Link | VPC endpoints (PrivateLink) |
| Application Insights | CloudWatch / X-Ray (ADOT) |
| Azure Database for PostgreSQL | RDS / Aurora PostgreSQL |
| Azure Cache for Redis | ElastiCache |
| Blob Storage | S3 |

## Week 3: エージェントを AWS に載せる

**目的:** 既存の強み(LangGraph / ADK でのエージェント基盤)を Strands + AgentCore に移植し、`create → dev → deploy → invoke` を一周する。

- [ ] 内職: [Strands Agents ドキュメント](https://strandsagents.com/) のマルチエージェントパターン(Agents-as-Tools / Swarm / Graph / Workflow)を読み、LangGraph との対応を整理する
- [ ] 内職: AgentCore の構成要素(Runtime / Memory / Gateway / Identity / Observability / Evaluations / Policy)を、以前設計した基盤のどの部分に当たるか対応づける
- [ ] 夜: [AgentCore CLI](https://github.com/aws/agentcore-cli) を入れ、README-AWS の手順どおりに小さなエージェントを `deploy` / `invoke` する
- [ ] 夜: 「状態を持たない API サーバ + 外部に状態を保存する」設計(トークンで再開するパターン)を AgentCore Runtime / Memory 上で試す
- [ ] 夜: CloudWatch でトレースとログを確認する

**成果物:** 動くエージェントのリポジトリ(公開できる内容のみ)、LangGraph ↔ Strands の対応メモ

## Week 4: RAG と Evals、アウトプット

**目的:** 未経験の RAG と評価を押さえ、成果を外に出す。

- [ ] 内職: README-AWS の「Enterprise RAG Blueprint」と「LLM Systems Evaluation」を読み、RAG Triad(Faithfulness など)を自分の言葉で説明する
- [ ] 内職: [Anthropic: Evaluating AI Agents](https://www.anthropic.com/engineering/demystifying-evals-for-ai-agents) を読む
- [ ] 夜: Bedrock Knowledge Bases + S3 Vectors で小さな RAG を作る(公開ドキュメントを対象にする)
- [ ] 夜: [Strands Evals](https://strandsagents.com/docs/user-guide/evals-sdk/quickstart/) で Golden Dataset を 10〜20 件作り、評価を回す
- [ ] 夜: Week 1〜4 の学びを Zenn 記事にまとめる(例: 「Azure で作ったエージェント基盤の考え方を AWS で作り直す」)
- [ ] 内職: 面接で出そうなシステム設計・ケース問題の最終リハーサル

**成果物:** RAG + Evals のデモ、Zenn 記事 1 本

---

## 後回しにするもの

エアギャップ / ATO / CMMC、EKS の詳細、Spark の分散チューニング、ホワイトペーパー、Iceberg の深掘り。内定後、または志望先が必要とする場合に着手します。

## 週次チェック

毎週末に 5 分で振り返ります。

1. 今週の成果物はできたか
2. 面接で聞かれて詰まった点は何か(翌週のタスクに追加する)
3. 来週の面接予定に合わせて、時間配分を変える必要はあるか
