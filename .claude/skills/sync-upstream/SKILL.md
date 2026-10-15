---
name: sync-upstream
description: Fork 元 (pierpaolo28/Awesome-FDE-Roadmap) の変更を取り込み、README.md の差分を AWS 版 README-AWS.md に移植する。「upstream を取り込んで」「Fork 元の更新を反映」「AWS 版を同期」などで使う。
---

# sync-upstream

Fork 元の更新を `README.md` にマージし、その差分を `README-AWS.md` に AWS 向けに移植する手順。

## 前提ルール

- `README.md` はこの Fork では**絶対に手で編集しない**（upstream と常に同一に保つ）。AWS 向けの変更は `README-AWS.md` にだけ書く。
- `README-AWS.md` の先頭に `**Synced with upstream:** \`<hash>\` (<date>)` がある。これが前回同期した upstream のコミット。
- 移植には `README-AWS.md` 内の「GCP → AWS Translation Table」を使う。
- `README-AWS.md` の本文は英語（American English）。ユーザーとのやり取りは日本語。
- push とブランチの main へのマージは、ユーザーの確認を取ってから行う。

## 手順

### 1. 事前チェック

```bash
git status --short
git branch --show-current
```

- 作業ツリーに未コミットの変更（特に `README.md` / `README-AWS.md`）があれば止めてユーザーに確認する。
- `main` にいなければ `git switch main` してよいか確認する。

### 2. upstream remote の準備と fetch

```bash
git remote get-url upstream 2>/dev/null || git remote add upstream https://github.com/pierpaolo28/Awesome-FDE-Roadmap.git
git fetch upstream
```

### 3. 前回同期ポイントの特定

```bash
BASE=$(grep -oE 'Synced with upstream:\*\* `[0-9a-f]+`' README-AWS.md | grep -oE '[0-9a-f]{7,}')
git cat-file -e "$BASE^{commit}" && echo "base ok: $BASE"
```

- `BASE` が取れない／存在しないコミットなら `git merge-base main upstream/main` を代わりに使い、その旨をユーザーに伝える。
- `README.md` がローカルで改変されていないか確認する。`git diff $BASE HEAD -- README.md` が空でなければ止めて報告する（ルール違反の編集があるため）。

### 4. 差分の確認

```bash
git log --oneline $BASE..upstream/main
git diff --stat $BASE upstream/main
git diff $BASE upstream/main -- README.md
```

- 新しいコミットがなければ「取り込む変更はありません」と報告して終了。
- `README.md` 以外（`CONTRIBUTING.md`、`LICENSE` など）の変更も一覧で報告する。

### 5. 作業ブランチでマージ

```bash
git switch -c sync-upstream-$(date +%Y-%m-%d)
git merge --no-edit upstream/main
```

- `README.md` でコンフリクトが起きたら、ローカルで編集されていたということなので、`git merge --abort` して報告する。
- `CLAUDE.md` や `.claude/` などこの Fork だけにあるファイルのコンフリクトは、Fork 側の内容を残す方向でユーザーに確認する。

### 6. README-AWS.md への移植

`git diff $BASE upstream/main -- README.md` の各 hunk を次のように分類して処理する。

| 種類 | 対応 |
| :--- | :--- |
| クラウド非依存（コンサル、書籍、ポッドキャスト、エアギャップのコンプライアンス、誤字修正など） | `README-AWS.md` の対応箇所にほぼそのままコピー |
| GCP 固有（GKE、BigQuery、ADK、Agent Platform など） | Translation Table で AWS の対応物に置き換える。AWS 側に同等品がなければ、無理に作らず「AWS には単一の同等品がない」と書くか、移植を見送ってユーザーに報告 |
| 新しい GCP 概念で表にない | AWS の対応物を調べて Translation Table に行を追加 |
| 見出しの追加・改名・並べ替え | `README-AWS.md` でも同じ構成にし、Table of Contents とアンカーを更新（アンカーは見出しテキストから GitHub の slug を導出する） |
| upstream で削除されたもの | AWS 版の対応箇所も削除するか、AWS 版独自に価値があるなら残してユーザーに確認 |

移植時の注意:
- AWS のサービス名・機能・提供状況（GA/Preview、提供終了、改名）は**記憶に頼らず WebSearch で現時点の情報を確認**する。改名済みのサービスは `New Name (formerly Old Name)` 形式で書く。
- 新規エントリは CONTRIBUTING.md の形式に従う: `*   **[Name](url):** One-sentence description of why an FDE should read/watch/use it.`
- ベンダーの宣伝的な表現は避ける。有料リソースは明記する。

### 7. 同期ハッシュの更新

```bash
NEW=$(git rev-parse --short upstream/main)
```

`README-AWS.md` 先頭の `Synced with upstream:` 行を `` `$NEW` (YYYY-MM-DD) `` に更新する。

### 8. リンクチェック

`README-AWS.md` で今回追加・変更したリンクが HTTP 200 で開けるか確認する。

```bash
grep -oE '\]\(https?://[^)]+\)' README-AWS.md | sed -E 's/^\]\(//; s/\)$//' | sort -u > "$TMPDIR/links.txt"
while read u; do (c=$(curl -sL -o /dev/null -w "%{http_code}" -A "Mozilla/5.0" --max-time 20 "$u"); echo "$c $u") & done < "$TMPDIR/links.txt"; wait
```

（一時ファイルはセッションの scratchpad に置く。200 以外のリンクは差し替えるか報告する。）

### 9. コミットと報告

```bash
git add README-AWS.md
git commit -m "Port upstream changes ($BASE..$NEW) to AWS edition"
```

ユーザーに日本語で報告する:
- 取り込んだ upstream コミットの一覧
- `README-AWS.md` に移植した内容（そのままコピー／AWS に翻訳／見送り を分けて）
- Translation Table に追加した行
- リンクチェックの結果

最後に、`main` へのマージ（`git switch main && git merge --ff-only sync-upstream-...`）と `git push origin main` をしてよいか確認する。
