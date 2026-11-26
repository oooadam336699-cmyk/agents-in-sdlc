---
title: "レッスン 1 - カスタム指示 (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: Agent mode →][next-lesson]

生成 AI を使うときは、コンテキストが重要です。タスクを特定の方法で進める必要がある場合や、Copilot に知っておいてほしい背景情報がある場合、その情報を参照できるようにします。Copilot と特定のコンテキストを共有する方法はいくつかあります。中でも重要なのが[指示ファイル][instruction-files]です。指示ファイルを使うと、コード生成の指針を伝えられます。

このレッスンでは、次のことを行います。

- リポジトリのカスタム指示とパス別の指示ファイルを使い、プロジェクト固有のコンテキスト、コーディング ガイドライン、ドキュメント標準を Copilot に伝える方法を確認します。
- 現在の指示を使って、フィルタリング機能の最初のデータ スライス（publisher ヘルパー）を生成します。
- `.github/copilot-instructions.md` にリポジトリ全体に適用する標準を追加します。
- 同じプロンプトをもう一度実行し、生成コードに新しい標準が反映されることを確認します。
- 指示の変更とフィルタリング機能を `main` にコミットし、次のレッスンで Copilot が最新の指針を使えるようにします。

> [!CAUTION]
> 生成コードは、設定した標準から外れる場合があります。Copilot の出力は確定的ではありません。この演習では、指示を更新した後の動作の**傾向**を確認します。出力が文字単位で同じになることを期待しないでください。

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

## 変更を作成するブランチを用意する

ここではベスト プラクティスに従い、変更用のブランチを作成します。

1. 前の演習で使った Codespace に戻ります。
2. <kbd>Control</kbd>+<kbd>`</kbd>（Mac）または <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux）を押して、新しいターミナルを開きます。
3. 新しいブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions
   ```

## 指示を更新する前に Copilot Chat を使う

> [!TIP]
> **Copilot Chat を開く**
>
> 以下の演習を始める前に Codespace に戻り、Copilot Chat パネルを開き、**New Chat** を選択して新しい会話を始めます。モードとモデルの選択は演習ごとに異なります。該当する手順で説明します。

カスタム指示の効果を確認するため、まず現在の指示ファイルを使ってプロンプトを送信します。その後、指示を更新して同じプロンプトをもう一度送信し、違いを確認します。

1. 前の演習のエディター タブを閉じ、Copilot が必要なコンテキストだけを取得するようにします。
2. `src/lib/publishers.ts` を開き、ヘルパー関数の配置場所を Copilot に示します。
3. Chat ビューのエージェント選択メニューから **Agent** を選択し、Copilot がファイルを変更できるようにします。

   ![Chat ビューのエージェント選択メニュー](../../../_images/shared-chat-mode-selector.png)

4. 次のプロンプトを送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot はプロジェクトを調べてコードを更新します。ヘルパー ファイルとテストなど、複数のファイルに変更が及ぶことがあります。
6. 提案されたヘルパーは、最初の引数に型付きの `db` クライアントを受け取り、publisher の型付き配列を返す関数です。これは `src/lib/*.ts` に適用される `.github/instructions/drizzle.instructions.md` のデータ層規則によるものです。
7. 提案されたコードに TSDoc doc comment とファイル レベルのコメント ヘッダーが**ない**ことを確認します。

> [!CAUTION]
> Copilot の出力は確率的なため、指示がなくても doc comment が追加される場合があります。その場合も問題ありません。指示を更新した後に動作が一貫することが、この演習の目的です。

## リポジトリ全体に適用する標準を追加する

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

## プロンプトを再実行して変化を確認する

1. Copilot Chat に戻り、**New Chat** を選択して会話をクリアします。
2. `src/lib/publishers.ts` を選択し、Copilot が適切なファイルに注目するようにします。
3. 先ほどと**同じプロンプト**を送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. 提案されたファイルの先頭に、次のようなコメント ブロックが追加されたことを確認します。

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

5. 提案された関数に、次のような TSDoc comment が追加されたことを確認します。

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

新しいプロジェクト標準を Copilot に伝え、次の演習で使う実際のコードに適用できました。

## 指示ファイルをコミットしてブランチをプッシュする

指示ファイルもリポジトリのほかのファイルと同じように、ソース管理で管理します。指示の更新とヘルパーの変更をコミットして、リポジトリにプッシュしましょう。

1. <kbd>Control</kbd>+<kbd>`</kbd>（Mac）または <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux）を押して、新しいターミナル ウィンドウを開きます。
2. ターミナルから `git status` を実行し、指示の更新とヘルパーの変更があることを確認します。

   ```bash
   git status
   ```

3. 指示の更新と、ヘルパーの基盤作成のために Copilot が変更したファイルをステージしてコミットします。

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # git status に追加の変更（たとえばテスト）が表示された場合は、それらのファイルも追加します。
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. ブランチをリポジトリにプッシュします。

   ```bash
   git push -u origin custom-instructions
   ```

## プルリクエストを作成してマージする

ブランチをプッシュしたら、カスタム指示の Issue に関連付けたプルリクエストを作成します。手動で行う代わりに、Copilot の GitHub ツールを使って Issue を探し、PR を作成してマージするよう依頼します。

> [!NOTE]
> Copilot に GitHub 上の操作を依頼するのは今回が初めてです。このプロジェクト テンプレートでは、GitHub ツールがすでにワークスペースに接続されています。Copilot が初めてツールを使う際、VS Code で GitHub へのサインインを求められることがあります。指示に従って認証してください。後のレッスンでは、この接続の仕組みである Model Context Protocol について学びます。

1. Codespace の Copilot Chat を開きます。
2. <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd>（Mac）または <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd>（Windows/Linux）を押して Copilot Chat ビューを開き、エージェント選択メニューで **Agent** が選択されていることを確認します。
3. 指示ファイルの更新に関する Issue を探し、現在のブランチから PR を作成するよう Copilot に依頼します。指示の変更と publisher ヘルパーの基盤作成の両方を説明するよう伝えます。

   ```plaintext
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot が Issue を探し、PR を作成します。
5. Copilot が GitHub 操作の許可を求めたら、コマンドを確認して、適切であれば **Allow** を選択します。
6. PR が作成されたら、PR をマージしてブランチを `main` に戻すよう、次のプロンプトを送ります。

   ```plaintext
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. Copilot が GitHub 操作やシェル コマンドの実行許可を求めたら、内容を確認して、適切であれば **Allow** を選択します。

GitHub Copilot の助けを借りてプルリクエストを作成し、マージできました。

## まとめと次のステップ

このプロジェクトで Copilot が指示ファイルからコンテキストを得る方法を確認し、VS Code の Copilot Chat で次のことを行いました。

- コード生成プロンプトを送信し、既存の指示で Copilot が何を提案するか確認しました。
- `.github/copilot-instructions.md` にリポジトリ全体に適用する標準を追加しました。
- 同じプロンプトをもう一度送信し、提案コードに新しい標準が反映されることを確認しました。
- 次のレッスンでフィルタリング機能の基盤として使う指示とヘルパーを `main` にコミットしました。

次は [agent mode][next-lesson] を使い、Copilot に複数のファイルを変更する新機能を追加してもらいます。

## リソース

- [GitHub Copilot のカスタマイズに使う指示ファイル][instruction-files]
- [カスタム指示を作成するためのベスト プラクティス][instructions-best-practices]
- [Copilot 向けのカスタム指示を改善する 5 つのヒント][copilot-instructions-five-tips]
- [GitHub Copilot の個人用カスタム指示][personal-instructions]
- [Awesome Copilot — 指示ファイルなどのリソース集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
