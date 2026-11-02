# 04 Datasets and Members on z/OS: Not Just Files and Folders

> Series: IBM Z Xplore — Fundamentals + Concepts | Challenge: FILES1 | Beginner, Fundamentals | 45 minutes
> Platform status: Completed (as of 2026-11-02)
> Prerequisite: VSC1. Source: IBM Z Xplore official FILES1 challenge


 documentation, plus my own practice.
> Language order in this article: English → 日本語 → 中文. The title stays in English.

---

## English

### What I did

FILES1 answers the most basic question on z/OS: where does data live? I expanded my Zowe dataset filter, built my own datasets with a setup job, and practiced renaming, deleting, copying, pasting, and creating members. I finished by submitting the validation job and getting a completion code of 0000.

### Steps I followed

1. Refined the DATA SETS filter to my own userid plus `ZXP.PUBLIC` — noting the comma and the period, which matter.
2. Noted that `ZXP.PUBLIC` datasets are read-only for everyone. If editing fails there, that is expected; real editing happens in my own datasets.
3. Submitted `PDSBUILD` from `ZXP.PUBLIC.JCL`. It created my `INPUT` partitioned dataset, my `SURPRISE` dataset, and my `SEQDS` sequential dataset. Submitting it again resets the challenge.
4. Renamed one member of `INPUT`, following the instruction written inside that member.
5. Deleted another member of `INPUT`, following its own instruction.
6. Copied a member from `SURPRISE` and pasted it into `INPUT` under its original name, so `INPUT` ended up with two members named after colors.
7. Added a record to `SEQDS` — a new line saying hello, with a note about the weather and a favorite cartoon — and saved it.
8. Created a new member in `INPUT` named `MYNEWMBR`. Member names are 1–8 characters: letters, digits, `@`, `#`, and `$` only.
9. Checked the result: `INPUT` should have 5 members (two original, one renamed, one created, one copied from `SURPRISE`), and the deleted member should be gone. Then I submitted `CHKFILES` from `ZXP.PUBLIC.JCL`. CC 0000 means success; CC 0127 means go back, fix a step, and resubmit.

### Key concepts

- **Partitioned Dataset (PDS) and member:** a PDS is like a library, and each member is like a book on the shelf. A COBOL source program is typically a member of a PDS. This connects directly to Issue #2, where I created a PDS in ISPF.
- **Sequential Dataset (PS / SEQDS):** one stream of records, with no members. Each record is a line. Sequential access is fastest when an application always reads records top to bottom, for example thousands of customer records in order.
- **Why not just files and folders?** Different access patterns need different structures. A PDS gives flexibility to fetch any member in any order; a sequential dataset gives speed for ordered processing. Choosing the right structure is part of keeping applications fast.
- **Dataset attributes:** beyond PDS vs. sequential, datasets carry attributes for performance, security, and scalability. In these challenges I mostly take the defaults, but a systems programmer needs to know what those values mean.

### Why this matters for COBOL

Almost every COBOL problem that looks like a program bug is actually a data problem: the wrong dataset, the wrong member, or the wrong record layout. Learning datasets before writing serious COBOL is not a detour; it is the prerequisite. When I can say “this program reads this member, with this record layout,” I can actually debug it.

My final INPUT members, as shown in the screenshot: GREEN, M4, M5, MYNEWMBR, PURPLE — five members in total, with the deleted member gone.

<img width="570" height="365" alt="Image" src="https://github.com/user-attachments/assets/9dc0cc2f-2788-4f6d-8c49-db2a09a43039" />

Next: 05 — JCL.

---

## 日本語

### やったこと

FILES1 は、z/OS における最も基本的な問い、「データはどこにあるのか」に答えるチャレンジです。Zowe のデータセットフィルターを広げ、セットアップジョブで自分のデータセットを作成し、メンバーの名前変更、削除、コピー、貼り付け、新規作成を練習しました。最後に検証ジョブをサブミットし、完了コード 0000 を取得しました。

### 手順

1. DATA SETS のフィルターを、自分のユーザーIDと `ZXP.PUBLIC` に広げました。カンマとピリオドの位置が重要です。
2. `ZXP.PUBLIC` のデータセットは全員が読み取り専用であることを確認しました。ここで編集が失敗しても想定内です。実際の編集は自分のデータセットで行います。
3. `ZXP.PUBLIC.JCL` から `PDSBUILD` をサブミットしました。これにより、自分の `INPUT`（区分データセット）、`SURPRISE`、`SEQDS`（順次データセット）が作成されました。再度サブミットすれば、チャレンジを最初からやり直せます。
4. `INPUT` のあるメンバーを、そのメンバー内に書かれた指示に従って名前変更しました。
5. 別のメンバーを、その指示に従って削除しました。
6. `SURPRISE` からメンバーをコピーし、元の名前のまま `INPUT` に貼り付けました。その結果、`INPUT` には色の名前のメンバーが 2 つできました。
7. `SEQDS` にレコードを 1 行追加しました。挨拶と天気、好きなアニメについての内容を書き、保存しました。
8. `INPUT` に `MYNEWMBR` という新しいメンバーを作成しました。メンバー名は 1〜8 文字で、英字、数字、`@`、`#`、`$` のみが使えます。
9. 結果を確認しました。`INPUT` には 5 つのメンバー（元の 2 つ、名前変更したもの、新規作成したもの、`SURPRISE` からコピーしたもの）があり、削除したメンバーは存在しないはずです。その後、`ZXP.PUBLIC.JCL` から `CHKFILES` をサブミットしました。CC 0000 が成功、CC 0127 は手順を見直して修正し、再サブミットする必要があるという意味です。

### 重要な概念

- **区分データセット（PDS）とメンバー：** PDS は図書館のようなもので、メンバーは本棚の本のようなものです。COBOL のソースプログラムは通常、PDS のメンバーとして格納されます。これは、ISPF で PDS を作成した Issue #2 と直接つながります。
- **順次データセット（PS / SEQDS）：** メンバーを持たない、1 本のレコードの流れです。各レコードが 1 行です。顧客レコードを何千件も上から順に読むような処理では、順次アクセスが最も高速です。
- **なぜファイルとフォルダだけでは不十分なのか：** アクセス方法が違えば、適した構造も違います。PDS は任意のメンバーを任意の順序で取り出せる柔軟性があり、順次データセットは順序処理の速さがあります。適切な構造を選ぶことが、アプリケーションを高速に保つ仕事の一部です。
- **データセット属性：** PDS か順次か以外にも、データセットには性能、セキュリティ、拡張性に関する属性があります。このチャレンジではほとんどデフォルト値を使いますが、システムプログラマーはそれらの意味を理解する必要があります。

### COBOL にとってなぜ重要か

プログラムのバグに見える COBOL の問題の多くは、実際にはデータの問題です。間違ったデータセット、間違ったメンバー、間違ったレコードレイアウトが原因です。本格的な COBOL を書く前にデータセットを学ぶことは、遠回りではなく前提条件です。「このプログラムは、このメンバーを、このレコードレイアウトで読む」と言えるようになれば、実際にデバッグできます。

最終的な INPUT のメンバーは、スクリーンショットの通り GREEN、M4、M5、MYNEWMBR、PURPLE の 5 つで、削除したメンバーはありません。

次回：05 — JCL について。

---

## 中文

### 我做了什么

FILES1 回答的是 z/OS 上最基本的问题：数据到底存在哪里？我扩大了 Zowe 的数据集过滤器，用一个配置作业建好自己的数据集，然后练习了成员的改名、删除、复制、粘贴和新建，最后提交验证作业，拿到完成码 0000。

### 操作步骤

1. 把 DATA SETS 过滤器扩大到自己的用户 ID 加上 `ZXP.PUBLIC`，注意逗号和句点的位置，这两处不能错。
2. 确认 `ZXP.PUBLIC` 的数据集所有人都是只读的，在那里改不掉是正常的，真正的编辑要在自己的数据集里做。
3. 提交 `ZXP.PUBLIC.JCL` 里的 `PDSBUILD`，它会生成我的 `INPUT` 分区数据集、`SURPRISE` 数据集和 `SEQDS` 顺序数据集。想重来就再提交一次它。
4. 按某个成员里写的指示，把 `INPUT` 的一个成员改名。
5. 按另一个成员里的指示，把它删除。
6. 从 `SURPRISE` 复制一个成员，以原名粘贴到 `INPUT`，这样 `INPUT` 里就有了两个以颜色命名的成员。
7. 给 `SEQDS` 加一条记录：新起一行打招呼，写写天气和最喜欢的动画，保存。
8. 在 `INPUT` 里新建一个成员，名字是 `MYNEWMBR`。成员名只能是 1–8 个字符：字母、数字、`@`、`#`、`$`。
9. 核对结果：`INPUT` 应该有 5 个成员（原来 2 个、改名 1 个、新建 1 个、从 `SURPRISE` 复制 1 个），被删的那个不应该在。然后提交 `ZXP.PUBLIC.JCL` 里的 `CHKFILES`，CC 0000 是成功，CC 0127 是有步骤没做对，回去改好再提交。

### 关键概念

- **分区数据集（PDS）与成员：** PDS 像书库，成员像书架上的书。COBOL 源码通常就是 PDS 里的一个成员，这和我在 Issue #2 里用 ISPF 建 PDS 是一回事。
- **顺序数据集（PS / SEQDS）：** 一条没有成员的记录流，每条记录就是一行。像成千上万条客户记录从上往下顺序读的场景，顺序访问最快。
- **为什么不直接用文件和文件夹？** 访问方式不同，适合的结构就不同。PDS 的好处是任何成员都能按任意顺序取用，顺序数据集的好处是顺序处理快。选对结构，本身就是让应用跑得快的一部分工作。
- **数据集属性：** 除了分区还是顺序，数据集还有一堆影响性能、安全和扩展性的属性。这些挑战里大多用默认值，但真正的系统程序员要知道这些值是什么意思。

### 对以后学 COBOL 有什么用

很多看起来像程序 bug 的 COBOL 问题，其实是数据问题：数据集错了、成员错了、记录布局错了。先学数据集再写正经 COBOL，不是绕路，而是前提。能说出“这个程序读的是这个成员、按这个记录布局读”，才真的能调试。

最后 INPUT 里的成员以截图为准：GREEN、M4、M5、MYNEWMBR、PURPLE，共 5 个，被删的那个不在。

下一篇：05 — JCL。
