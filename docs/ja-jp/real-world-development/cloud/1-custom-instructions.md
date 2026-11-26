---
title: "レッスン 1 - カスタム指示 (Cloud agent)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: Copilot cloud agent →][next-lesson]

生成 AI を使うときは、コンテキストが重要です。タスクを特定の方法で進める必要がある場合や、Copilot に知っておいてほしい背景情報がある場合、その情報を参照できるようにします。[指示ファイル][instruction-files]を使うと、Copilot に何をしてほしいかだけでなく、どのように作業してほしいかも伝えられます。

このレッスンでは、次のことを行います。

- リポジトリのカスタム指示とパス別の指示ファイルを使い、プロジェクト固有のコンテキスト、コーディング ガイドライン、ドキュメント標準を Copilot に伝える方法を確認します。
- `.github/copilot-instructions.md` にリポジトリ全体に適用する標準を追加します。

> [!NOTE]
> VS Code と CLI のハーネスとは異なり、このレッスンでは変更前後のプロンプトを比較しません。Copilot cloud agent は GitHub Issue に対して非同期で動作するため、リアルタイムの比較が難しいためです。このハーネスの後半で、cloud agent が作成したプルリクエストをレビューするときに、指示ファイルの影響を確認します。

## 指示ファイル

### シナリオ

Tailspin Toys には、開発プロセスに関するガイドラインと要件があります。たとえば、次のとおりです。

- データ層には必ず unit test を用意します。
- UI はダーク モードにし、現代的な見た目にします。
- コードには TSDoc doc comment 形式でドキュメントを追加します。
- 各ファイルの先頭に、そのファイルの役割を説明するコメント ブロックを追加します。

指示ファイルを使って Copilot に必要な情報を伝え、これらの開発慣行に沿った作業を促します。

### カスタム指示

カスタム指示を使うと、コーディング スタイルや要件などのコンテキストを Copilot に伝えられます。これにより、より適切な提案やコード スニペットを得やすくなります。推奨するコーディング規則やライブラリ、コメントの形式などを指定できます。リポジトリ全体に適用する指示も、特定のタスクやファイルだけに適用する指示も作成できます。

指示ファイルには次の 2 種類があります。

- `.github/copilot-instructions.md` は、リポジトリに関する**すべての**要求で Copilot に送信される単一の指示ファイルです。技術スタック、開発内容の概要、ベスト プラクティスなど、Copilot Chat や CLI でのほとんどの要求に関係するプロジェクト レベルの情報を記載します。
- `.github/instructions/*.instructions.md` は、特定のタスクやファイル種別に適用する指示ファイルです。TypeScript や Astro などの言語、新しい UI コンポーネントの作成、unit test の追加などに関するガイドラインを記載できます。

> [!NOTE]
> IDE では、指示ファイルは Copilot Chat のコード生成にのみ使われます。コード補完や次の編集候補には使われません。
>
> Copilot Chat、Copilot CLI、Copilot cloud agent は、リポジトリ レベルの指示ファイルと、`applyTo` フロントマターで対象が指定された `*.instructions.md` ファイルを使用します。
>
> Copilot は AGENTS.md や CLAUDE.md など、ほかの標準の指示ファイルにも対応しています。[対応する指示ファイルの形式][custom-instructions-support]を参照してください。

### 指示ファイルを管理するためのベスト プラクティス

このワークショップでは、指示ファイルの作成方法をすべて説明するわけではありません。サンプル プロジェクトには代表的な構成を示しています。基本的には、次の点を意識してください。

- `copilot-instructions.md` には、プロジェクトの説明、構成、全体的なコーディング標準など、プロジェクト レベルの指針を記載します。
- `*.instructions.md` には、unit test、Astro コンポーネント、データ層など、特定のファイル種別やタスクに関する指示を記載します。
- 自然な言葉で、明確なガイダンスとコードの良い例・悪い例を示します。

指示ファイルの作り方は一つではありません。プロジェクトで試しながら、最適な使い方を見つけてください。

> [!TIP]
> GitHub Copilot を使うプロジェクトには、十分な指示ファイルを用意することをお勧めします。このプロジェクトには、[UI の更新][ui-instructions]や [Astro][astro-instructions] など、さまざまなタスク向けの指示ファイルがあります。
>
> Copilot を使って指示ファイルを生成することもできます。操作方法は利用する環境によって異なります（たとえば、VS Code の **Configure Chat → Generate Agent Instructions** や Copilot CLI の `/init`）。該当するレッスンで説明します。
>
> テンプレートやひな形を探している場合は、指示ファイル、カスタム エージェント、その他のリソースをまとめた [awesome-copilot][awesome-copilot] を確認してください。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## このプロジェクトのカスタム指示ファイルを確認する

このリポジトリに含まれる指示ファイルを読みましょう。中核となる `copilot-instructions.md` と、さまざまなタスク向けの `*.instructions.md` ファイルがあります。エディターまたは GitHub の Web UI で開いてください。

1. `.github/copilot-instructions.md` を開きます。
2. プロジェクトの簡単な説明と、**Agent notes**、**Code standards**、**Scripts**、**Repository Structure** などのセクションを確認します。**Code standards** の下には **GitHub Actions Workflows** の指針もあります。これらは Copilot を使うあらゆる作業に適用されます。
3. `.github/instructions` フォルダーを開いて内容を確認します。Astro ファイル、Drizzle データ層、テストなどの指示があります。
4. `.github/instructions/unit-tests.instructions.md` を開きます。先頭の `applyTo` フィールドが、リポジトリのルートからの glob パターンで適用対象を指定しています。この例では、`**/*.test.ts` に一致する TypeScript のテスト ファイルが対象です。
5. このプロジェクトの unit test 作成に関する指示を確認します。
6. 最後に `.github/instructions/drizzle.instructions.md` を開き、末尾までスクロールします。`unit-tests.instructions.md` などの指示ファイルや、既存ファイルへのリンクがあります。大きな指示を小さな再利用可能なファイルに分け、コード生成時の参考例を指定できます（パスはリポジトリのルートではなく、指示ファイルからの相対パスです）。

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** セクションでは、このプロジェクトのコーディング標準を説明していますが、コード内のドキュメントはまだ必須ではありません。次の手順で TSDoc doc comment とファイル ヘッダーの規則を追加します。

## リポジトリ全体に適用する標準を追加する

ここで編集するのは、プロジェクト全体に適用する TSDoc doc comment とファイル レベルのコメント ヘッダーの規則です。後続のレッスンで Issue を割り当てたとき、cloud agent がこの規則を使います。

編集の前に、作業用ブランチを作成します（cloud agent は Issue の対象ブランチから指示を読み込みますが、変更を整理してコミットします）。

1. Codespace のターミナルで、新しいブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions-cloud
   ```

前の説明のとおり、`.github/copilot-instructions.md` はプロジェクト レベルの情報を Copilot に伝えるファイルです。コード提案の質を高めるため、コーディング標準を記載しましょう。

1. `.github/copilot-instructions.md` を再度開きます。
2. 27 行目付近にある **Code formatting requirements** セクションを探します。現在のコーディング標準が記載されていますが、コード内ドキュメントの規則はありません。そのため、生成されたヘルパー関数にも doc comment がありませんでした。
3. 既存の標準のすぐ下に次の Markdown 行を追加し、ファイル ヘッダーと TSDoc doc comment を Copilot に指示します。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. `copilot-instructions.md` を保存します。

> [!TIP]
> 前のレッスンで見たように、指示ファイルは `.github/copilot-instructions.md` にリポジトリ全体の指示として置くことも、特定の言語、ファイル種別、タスク向けの `*.instructions.md` として作成することもできます。今回追加する doc comment の規則のような、プロジェクト全体の標準にはリポジトリ レベルのファイルが適しています。

## 指示の変更をコミット、プッシュ、マージする

cloud agent は、Issue の対象ブランチにある指示ファイルを読み込みます。次のレッスンで Copilot に Issue を割り当てると、`main` からブランチが作成されます。指示を読み込ませるには、変更を `main` に反映してください。

1. ステージしてコミットします。

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. ブランチをプッシュします。

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. github.com で `custom-instructions-cloud` から `main` へのプルリクエストを作成してマージします。次のレッスンで Issue を割り当てると、cloud agent はこの指示を読み込みます。

> [!TIP]
> このワークショップで直接 `main` を使いたい場合は、ブランチ作成を省略して `main` に直接コミットしてもかまいません。実際のプロジェクトでの手順に近づけるため、ここではブランチを使っています。

## まとめと次のステップ

このプロジェクトで Copilot が指示ファイルからコンテキストを得る方法を確認し、`.github/copilot-instructions.md` にリポジトリ全体の標準を追加しました。このハーネスで cloud agent が作成するプルリクエストを確認するときに、この標準が適用されているかを確認します。

次は、[Copilot cloud agent に初めて Issue を割り当てます][next-lesson]。

## リソース

- [GitHub Copilot のカスタマイズに使う指示ファイル][instruction-files]
- [カスタム指示を作成するためのベスト プラクティス][instructions-best-practices]
- [Copilot 向けのカスタム指示を改善する 5 つのヒント][copilot-instructions-five-tips]
- [Awesome Copilot — 指示ファイルなどのリソース集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
