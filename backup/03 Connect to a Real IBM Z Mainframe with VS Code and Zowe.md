# 03 Connect to a Real IBM Z Mainframe with VS Code and Zowe

> Series: IBM Z Xplore — Fundamentals + Concepts | Challenge: VSC1 | Beginner, Fundamentals | 45 minutes
> Platform status: Completed (as of 2026-11-02)
> Source: IBM Z Xplore official VSC1 challenge documentation, plus my own practice on the real system.
> Language order in this article: English → 日本語 → 中文. The title stays in English.

---

## English

### What I did

VSC1 is the first step from “learning about mainframes” to actually working on one. I installed the tooling, created a Zowe connection profile in VS Code, and submitted a validation job on a real IBM Z system. This is different from my earlier work in Issue #1, where I simulated a mainframe locally with Hercules and TK5. Here the host is real, and the editor is VS Code.

### How I got connected

1. Installed Node.js (LTS), a Java runtime, and VS Code.
2. Installed the IBM Z Open Editor extension, which brings in Zowe Explorer.
3. Created the connection profile and edited `zowe.config.json` — this is the core of the challenge, detailed in the next section.
4. Added my own Z userid and password through “Manage Profile → Update Credentials.” I do not publish credentials here.
5. Set three filters in Zowe Explorer: DATA SETS to my own userid, USS to my home directory, and JOBS to my userid as owner.
6. Opened `ZXP.PUBLIC.JCL`, found the member `CHKVSC`, and submitted it as a job. When the validation job succeeded, VSC1 was marked Completed.

### The core of VSC1: creating the profile and editing `zowe.config.json` (tutorial pp. 13–18)

This is where most of the work — and most of the possible mistakes — actually sit, so it is worth going through slowly.

**Step 6 — Click the + next to DATA SETS.** In the Zowe Explorer view, the + sign at the top of the DATA SETS section is how a new connection starts. Clicking it is the entry point to everything that follows; without a profile, there is no dataset view, no USS view, and no job view.

<img width="1448" height="890" alt="Image" src="https://github.com/user-attachments/assets/e7f83507-5fd8-499a-bba6-b9ba3fc0d45e" />


**Step 7 — Create a Team Configuration file, and choose Global.** The prompt offers “Create a new connection to z/OS” and “Create a new Team Configuration file.” Choose the Team Configuration option.

<img width="1386" height="506" alt="Image" src="https://github.com/user-attachments/assets/dc7ae616-0626-4ba0-a1c3-a3d3602a8ae3" />

 If a default Team Configuration with a profile called `zosmf` was already installed, that can be used instead, with the same edits below. Next, choose the location: **Global** (in the Zowe home directory), not Project. 

<img width="1330" height="534" alt="Image" src="https://github.com/user-attachments/assets/3e1ec613-b5d5-4b27-a925-8f8191607550" />

The tutorial warns that the VS Code prompt window can disappear if you switch to another application without selecting an option, so it is easier to line the windows up side by side than to keep switching back and forth.



**Editing `zowe.config.json` — four changes, then save.** An edit session opens on `zowe.config.json`. At the top are the `zosmf` and `tso` sections, and near the bottom is the `base` section. The four changes are:

- In the `zosmf` section, set `port` to **10443**.
- In the `tso` section, set `account` to **FB3**.
- In the `base` section, set `host` to the IBM Z Xplore server address given in the challenge.
- In the `base` section, set `rejectUnauthorized` to **false**, because the server uses a self-signed certificate that Zowe would otherwise reject.

After editing, save the file (File → Save). An unsaved config file is one of the easiest mistakes to make here: everything looks right, but nothing has actually been applied yet.

**My edited file.** The screenshot below is my own `zowe.config.json` after editing, with the four changed values marked. This is the file that my connection actually runs on.

<img width="955" height="1093" alt="Image" src="https://github.com/user-attachments/assets/9aa61461-185c-4b57-9bb9-740c984fda6d" />


### What I learned, including the pitfalls

- A connection problem is usually a profile problem, not a mainframe problem: a wrong credential, a wrong port, or an unsaved config file. The tutorial’s advice is practical — re-enter credentials through Update Credentials, delete and recreate a broken profile if needed, and when in doubt, restart VS Code.
- The dataset list entry that looks exactly like your userid is not a dataset you can open. Closing that error and moving on is the right response, not a sign that you broke anything.
- The red wavy line under JCL in the editor is an editor configuration warning. It does not stop you from submitting the job.
- Apart from getting the profile and config values right, the connection itself went smoothly, with no other obstacle worth noting.

### Why this matters for COBOL

COBOL source lives in datasets, and COBOL programs are run as jobs. If I cannot connect reliably, browse datasets, and submit a job from my editor, every later COBOL step becomes friction. VSC1 removes that friction, and it matches how modernization teams work: the mainframe stays, but the tooling around it becomes modern.

Next: 04 — Datasets and Members on z/OS.

---

## 日本語

### やったこと

VSC1 は、「メインフレームについて学ぶ」段階から、実際にメインフレームで作業する段階への第一歩です。ツールをインストールし、VS Code に Zowe の接続プロファイルを作成し、実際の IBM Z システムで検証ジョブをサブミットしました。Issue #1 で Hercules と TK5 を使ってローカルでシミュレーションしたのとは違い、ここではホストは本物で、エディタは VS Code です。

### 接続までの手順

1. Node.js（LTS）、Java ランタイム、VS Code をインストールしました。
2. IBM Z Open Editor 拡張機能をインストールしました。これにより Zowe Explorer が導入されます。
3. 接続プロファイルを作成し、`zowe.config.json` を編集しました。これがこのチャレンジの中心で、次の節で詳しく説明します。
4. 「Manage Profile → Update Credentials」から、自分の Z ユーザーID とパスワードを登録しました。認証情報はここでは公開しません。
5. Zowe Explorer で 3 つのフィルターを設定しました。DATA SETS は自分のユーザーID、USS はホームディレクトリ、JOBS は所有者を自分のユーザーIDにしました。
6. `ZXP.PUBLIC.JCL` を開き、メンバー `CHKVSC` を見つけてジョブとしてサブミットしました。検証ジョブが成功し、VSC1 は Completed になりました。

### VSC1 の中心：プロファイルの作成と `zowe.config.json` の編集（チュートリアル pp. 13–18）

作業の大部分、そしてミスの可能性の大部分はここにあります。だからこそ、ゆっくり確認する価値があります。

**ステップ 6 — DATA SETS の横の + をクリックする。** Zowe Explorer ビューで、DATA SETS セクションの上部にある + 記号が、新しい接続の出発点です。これをクリックすることが、その後のすべての入口になります。プロファイルがなければ、データセットビューも、USS ビューも、ジョブビューもありません。


**ステップ 7 — Team Configuration ファイルを作成し、Global を選ぶ。** プロンプトには「Create a new connection to z/OS」と「Create a new Team Configuration file」が表示されます。Team Configuration の方を選びます。`zosmf` というプロファイルを持つデフォルトの Team Configuration がすでにインストールされている場合は、それを使い、同じ編集を行ってもかまいません。次に保存場所を選びます。**Global**（Zowe ホームディレクトリ）で、Project ではありません。チュートリアルでは、別のアプリケーションに切り替えて選択せずにいると、VS Code のプロンプトウィンドウが消えてしまうことがあると警告しています。そのため、ウィンドウを切り替えるよりも、並べて表示する方が楽です。



**`zowe.config.json` の編集 — 4 か所の変更、そして保存。** `zowe.config.json` の編集セッションが開きます。上部に `zosmf` と `tso` のセクションがあり、下部近くに `base` セクションがあります。変更は 4 か所です。

- `zosmf` セクションで、`port` を **10443** にします。
- `tso` セクションで、`account` を **FB3** にします。
- `base` セクションで、`host` をチャレンジで指定された IBM Z Xplore サーバーのアドレスにします。
- `base` セクションで、`rejectUnauthorized` を **false** にします。サーバーが自己署名証明書を使っているため、そうしないと Zowe に拒否されるからです。

編集が終わったら、ファイルを保存します（File → Save）。保存忘れは、ここで最も起こしやすいミスの 1 つです。すべて正しく見えても、実際には何も適用されていない、という状態になります。

**自分の編集済みファイル。** 下のスクリーンショットは、編集後の自分の `zowe.config.json` で、変更した 4 か所がマークされています。これが、自分の接続が実際に動作しているファイルです。


### 学んだこと、つまずきやすい点

- 接続の問題は、多くの場合メインフレームの問題ではなくプロファイルの問題です。認証情報の入力ミス、ポートの間違い、設定ファイルの保存忘れなどが原因になります。対処法は実践的です。Update Credentials で再入力する、壊れたプロファイルは削除して作り直す、それでも直らなければ VS Code を再起動する、という手順です。
- データセット一覧に自分のユーザーIDとまったく同じ名前の項目が表示されても、それは開けるデータセットではありません。エラーを閉じて次に進むのが正しい対応です。
- エディタで JCL の下に表示される赤い波線は、エディタの設定に関する警告で、ジョブのサブミットを妨げるものではありません。
- プロファイルと設定値を正しくすること以外に、接続自体は順調で、特記すべき障害はありませんでした。

### COBOL にとってなぜ重要か

COBOL のソースはデータセットに格納され、COBOL プログラムはジョブとして実行されます。エディタから確実に接続し、データセットを参照し、ジョブをサブミットできなければ、その後の COBOL の学習はすべて手間が増えます。VSC1 はその手間を取り除きます。これは、メインフレームを残しながら周辺のツールをモダン化する、現場のモダナイゼーションの進め方とも一致します。

次回：04 — z/OS のデータセットとメンバーについて。

---

## 中文

### 我做了什么

VSC1 是从“了解大型机”到“真正上机操作”的第一步。我装好工具，在 VS Code 里建好 Zowe 连接配置，并在真实的 IBM Z 系统上提交了一个验证作业。这和我之前 Issue #1 里用 Hercules + TK5 在本地模拟不同：这次主机是真实的，编辑器是 VS Code。

### 连通的步骤

1. 安装 Node.js（LTS）、Java 运行环境和 VS Code。
2. 安装 IBM Z Open Editor 扩展，它会一并带入 Zowe Explorer。
3. 建好连接配置并改好 `zowe.config.json`，这是整个挑战的核心，下一节重点讲。
4. 通过“Manage Profile → Update Credentials”填入自己的 Z 用户 ID 和密码，凭证不在博客里公开。
5. 在 Zowe Explorer 里设好三个过滤器：DATA SETS 按自己的用户 ID，USS 按自己的主目录，JOBS 按自己为作业所有者。
6. 打开 `ZXP.PUBLIC.JCL`，找到成员 `CHKVSC` 并提交作业。验证作业成功后，VSC1 显示为 Completed。

### VSC1 的核心：建配置并改 `zowe.config.json`（教程第 13–18 页）

大部分工作、也最容易出错的地方都在这里，所以值得慢慢过一遍。

**第 6 步 — 点 DATA SETS 旁边的 + 号。** 在 Zowe Explorer 视图里，DATA SETS 顶部那个 + 号就是新连接的入口。点了它，后面才有数据集视图、USS 视图和作业视图；没有配置，这些视图都不存在。


**第 7 步 — 新建 Team Configuration 文件，位置选 Global。** 弹出的选项里有“Create a new connection to z/OS”和“Create a new Team Configuration file”，选后者。如果已经装了一个带 `zosmf` 配置的默认 Team Configuration，也可以直接用它，改同样的地方。接下来选保存位置：**Global**（Zowe 主目录），不是 Project。教程里提醒过，VS Code 的提示窗口如果切到别的程序又没选，可能会自己消失，所以把窗口并排摆着比来回切换省事。



**改 `zowe.config.json`：改 4 处，然后保存。** 打开的编辑会话里，上面是 `zosmf` 和 `tso` 两节，靠下是 `base` 一节。4 处改动是：

- `zosmf` 一节里，`port` 改成 **10443**。
- `tso` 一节里，`account` 改成 **FB3**。
- `base` 一节里，`host` 改成挑战给出的 IBM Z Xplore 服务器地址。
- `base` 一节里，`rejectUnauthorized` 改成 **false**，因为服务器用的是自签名证书，不改会被 Zowe 拒绝。

改完一定要保存（File → Save）。没保存是最容易犯的错：看着都对，但实际上什么都没生效。

**我改好的文件。** 下面这张就是我自己改完的 `zowe.config.json`，4 处改动都用红框标出来了，我的连接就是跑在这份文件上的。


### 学到的东西，包括容易踩的坑

- 连不上时，先怀疑配置而不是主机：凭证输错、端口不对、配置文件没保存，都是常见原因。教程给的办法很实用：用 Update Credentials 重新输入，坏掉的配置删了重建，实在不行就重启 VS Code。
- 数据集列表里那个和你用户 ID 一模一样的条目，并不是能打开的数据集，关掉报错继续做就行，不是你弄坏了。
- 编辑器里 JCL 下面的红色波浪线只是编辑器配置提示，不影响提交作业。
- 除了把配置和配置值改对，连接本身很顺利，没有其他值得特别说明的障碍。

### 对以后学 COBOL 有什么用

COBOL 源码放在数据集中，COBOL 程序要作为作业来运行。如果不能从编辑器里稳定地连接、浏览数据集、提交作业，后面每一步 COBOL 都会很别扭。VSC1 就是把这层摩擦去掉。这也和现实里的现代化改造思路一致：大型机留着，周边的工具先现代化。

下一篇：04 — z/OS 的数据集与成员。
