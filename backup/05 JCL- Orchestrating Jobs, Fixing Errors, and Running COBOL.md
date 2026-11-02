> Series: IBM Z Xplore — Fundamentals + Concepts | Challenge: JCL1 | Beginner, Fundamentals | 60 minutes
> Platform status: Completed (as of 2026-11-02)
> Prerequisite: FILES1. Source: IBM Z Xplore official JCL1 challenge documentation, plus my own practice.
> Language order in this article: English → 日本語 → 中文. The title stays in English.

---

## English

### What JCL is, in one paragraph

JCL is not really a programming language. It is a way of describing a task to z/OS so that the Job Entry Subsystem (JES) can gather the resources, schedule the work, and run it. Think of JCL as the order written up for a kitchen, and JES as the kitchen staff that decides how to handle it. On a mainframe you rarely just “run” a program; you submit a job, then read its output to find out what happened.

### Part 1 — Set up and find a job

1. Submitted `JCLSETUP` from `ZXP.PUBLIC.JCL`. It allocated the datasets I needed for this challenge, including my own `JCL` dataset. It uses the symbolic `&SYSUID.` — note the trailing period — which the system replaces with my own userid, so everyone can use the same job. Running it twice can cause DUPLICATE dataset errors, which is itself a useful lesson.
2. Set the JOBS filter: Owner = my userid, Prefix = `*`, Job ID blank. Found `JCLSETUP` there, opened its output, and confirmed CC 0000 in `JESMSGLG`. CC 0 means everything ran as expected; any other code is worth investigating.
3. Learned the shortcut: the popup message in the bottom-right of VS Code after submission usually jumps straight to the job output, instead of digging through the JOBS view. A job starts as ACTIVE; refreshing its twistie refreshes its status.

### Part 2 — My first copy job, and my first error

4. Copied the `JCL1` member from `ZXP.PUBLIC.JCL` into my own `JCL` dataset, because I would be modifying it. That job uses the `IEBGENER` utility to copy the `JCL2` member into my dataset.
5. Submitted it and got CC 0012. The joblog (`JES2:JESMSGLG`) showed a missing DD statement for `SYSIN`. IEBGENER needs `SYSPRINT` (messages), `SYSUT1` (source), `SYSUT2` (target), and `SYSIN` (control statements, or DUMMY when there are none and the output is sequential). I added the missing DD, saved, resubmitted, and got CC 0000 — and a new `JCL2` member (plus a `JCL3` member from the second step).

Key vocabulary here: **JOB** identifies the job, **EXEC** says which program to run, and **DD (Data Definition)** connects a program to its input and output datasets. A DD name in JCL must match what the program expects.

### Part 3 — Compile and run COBOL, then fix an ABEND

6. Opened my copy of `JCL2`. It compiles COBOL source and puts the resulting program in my `LOAD` dataset — those programs are binary and cannot be viewed in VS Code. The `//COBRUN` step compiles; the `//RUN EXEC PGM=CBL0001` step runs the program.
7. Submitted `JCL2` and got an ABEND (abnormal end), as the challenge warns you will. There is no utility manual to consult this time; the COBOL code itself is the documentation. I opened the COBOL source named by `//COBOL.SYSIN`, looked at its **FILE-CONTROL** section, and compared the file names used by the program with the DD names in the JCL — for example, the program’s `FIRST-NAME` file is linked to the `FNAMES` DD statement.
8. The mismatch was one letter short. The program was looking for `COMBINED`, but line 27 of my `JCL2` only had `//COMBINE DD DISP=SHR,DSN=&SYSUID..OUTPUT(NAMES)`. That is the line the editor flagged with “FIX THIS ONE”. The fix was simple: rename it to `//COMBINED`, and add a space after it so the columns still line up, giving `//COMBINED DD DISP=SHR,DSN=&SYSUID..OUTPUT(NAMES)`. Then save with Ctrl+S, right-click `JCL2`, and choose Submit Job. This time it came back CC 0000, with the output in the right member of my `OUTPUT` dataset.

<img width="1025" height="655" alt="Image" src="https://github.com/user-attachments/assets/cb2d1772-5aa1-4110-a849-cc05d423b1d7" />

The general rule I take from this: for a z/OS program to work with a dataset, four things must line up — the file definition inside the program, the actual dataset named in `DSN=`, a correct `DISP=`, and a DD statement whose name matches the program’s file definition.

### Part 4 — DISP, DUMMY, and getting the output exactly right

9. Opened `JCL3`. It uses `IEBGENER` in several steps to write records into a sequential dataset, with the source data given “in-stream” after the DD statement (`DD *` instead of `DSN=`). The content is a timetable: station stops from Poughkeepsie, NY to Grand Central Terminal in New York, plus header and operating-hours text.
10. Learned DISP properly: the first field is status — `NEW` (create), `SHR` (reuse, shareable), `OLD` (reuse, exclusive), `MOD` (append, or create if missing, sequential only). The second field is what happens on normal completion (`DELETE`, `CATLG`, `PASS`), and the third is what happens on failure.
11. Learned `DUMMY`: it means “this DD is required, but it will not actually be used this time, so do not waste time setting it up.” IEBGENER still requires `SYSIN` and `SYSPRINT` DDs even when their output is not needed.
12. Submitted `JCL3`, got CC 0000 — but CC 0000 only means nothing broke, not that the output is correct. Checking the output, I found two duplicates of my own making. First, `Beacon - 59mi` appeared twice in a row, on lines 52 and 53. The fix: keep line 52, delete line 53 entirely. Do not touch the `//BEACON EXEC PGM=IEBGENER` step on line 48 — that is code, not the duplicated data line. Second, the whole `//CORTLNDT` step was duplicated: the step starting at line 85 was repeated, word for word, at line 94. The fix: keep the first complete step, lines 85 to 92, and delete the entire duplicate block, lines 94 to 101. With those two fixes, plus the correct header and footer, the output was 10 station stops with no repeats, totaling 23 lines in `JCL3OUT`. I also had to delete `JCL3OUT` before each resubmission of `JCL3`.

<img width="1118" height="949" alt="Image" src="https://github.com/user-attachments/assets/b8102807-b8db-4877-b55b-e77b9536559d" />

<img width="1114" height="955" alt="Image" src="https://github.com/user-attachments/assets/db2b7674-264b-4939-9548-c5ff634bbaa6" />

13. Finally submitted `CHKJCL1` from `ZXP.PUBLIC.JCL` to validate both `JCL2` and `JCL3`. CC 0000 means done; CC 0127 means go back and recheck.
### Why this matters for COBOL

A COBOL program without JCL does not run in production. Compile, link, and run are themselves jobs described in JCL, and the DD statements are the bridge between the program’s FILE-CONTROL and the real datasets. JCL1 taught me the debugging loop I will use for every COBOL program from now on: submit, read the joblog, fix one thing, resubmit, and verify the output — not just the completion code.

Next: 06 — USS1 (outline only until completed).

---

## 日本語

### JCL とは、一言でいうと

JCL は厳密にはプログラミング言語ではありません。z/OS に対してタスクを記述する方法で、Job Entry Subsystem（JES）がリソースを集め、作業をスケジュールし、実行できるようにするものです。JCL をキッチンへの注文票、JES をその注文をどう処理するか決めるキッチンスタッフに例えると分かりやすいです。メインフレームでは、プログラムをただ「実行する」のではなく、ジョブをサブミットし、その出力を読んで何が起きたかを確認します。

### パート 1 — セットアップとジョブの確認

1. `ZXP.PUBLIC.JCL` から `JCLSETUP` をサブミットしました。これにより、このチャレンジで必要なデータセット（自分の `JCL` データセットを含む）が割り当てられました。ここではシンボリック `&SYSUID.` が使われています。末尾のピリオドに注意してください。システムがこれを自分のユーザーIDに置き換えるため、誰でも同じジョブを使えます。2 回実行すると DUPLICATE データセットのエラーになることがあり、それ自体も良い学びです。
2. JOBS のフィルターを設定しました。所有者は自分のユーザーID、プレフィックスは `*`、ジョブIDは空白です。そこで `JCLSETUP` を見つけ、出力を開き、`JESMSGLG` で CC 0000 を確認しました。CC 0 はすべて想定通りに実行されたことを意味し、それ以外のコードは調査する価値があります。
3. 近道も学びました。サブミット後に VS Code の右下に表示されるポップアップメッセージから、JOBS ビューを探し回らずに直接ジョブ出力へ移動できます。ジョブは最初 ACTIVE で、ツイストを閉じて開き直すと状態が更新されます。

### パート 2 — 初めてのコピージョブと、初めてのエラー

4. `ZXP.PUBLIC.JCL` から `JCL1` メンバーを自分の `JCL` データセットにコピーしました。これから変更するためです。このジョブは `IEBGENER` ユーティリティを使い、`JCL2` メンバーを自分のデータセットにコピーします。
5. サブミットすると CC 0012 になりました。ジョブログ（`JES2:JESMSGLG`）を見ると、`SYSIN` の DD 文が欠けていることが原因でした。IEBGENER には、`SYSPRINT`（メッセージ）、`SYSUT1`（コピー元）、`SYSUT2`（コピー先）、`SYSIN`（制御文、ない場合は DUMMY）が必要です。欠けていた DD を追加し、保存して再サブミットすると CC 0000 になり、新しい `JCL2` メンバー（第 2 ステップによる `JCL3` メンバーも）ができました。

ここでの基本用語は次の通りです。**JOB** はジョブを識別し、**EXEC** は実行するプログラムを指定し、**DD（Data Definition）** はプログラムと入出力データセットを結びつけます。JCL の DD 名は、プログラムが期待する名前と一致しなければなりません。

### パート 3 — COBOL のコンパイルと実行、そして ABEND の修正

6. 自分の `JCL2` を開きました。これは COBOL ソースをコンパイルし、結果のプログラムを自分の `LOAD` データセットに入れます。`LOAD` 内のプログラムはバイナリで、VS Code では表示できません。`//COBRUN` ステップがコンパイル、`//RUN EXEC PGM=CBL0001` ステップがプログラムの実行です。
7. `JCL2` をサブミットすると、チャレンジの予告通り ABEND（異常終了）になりました。今回は参照できるユーティリティのマニュアルはなく、COBOL コード自体がドキュメントです。`//COBOL.SYSIN` が指す COBOL ソースを開き、**FILE-CONTROL** セクションを見て、プログラムが使うファイル名と JCL の DD 名を比較しました。たとえば、プログラムの `FIRST-NAME` ファイルは、JCL の `FNAMES` DD 文に結びつけられています。
8. 原因は、1 文字足りないだけでした。プログラムが求めていたのは `COMBINED` なのに、自分の `JCL2` の 27 行目には `//COMBINE DD DISP=SHR,DSN=&SYSUID..OUTPUT(NAMES)` としか書いてありませんでした。エディタで「FIX THIS ONE」と印がついていたのもこの行です。直し方は単純で、`//COMBINED` に書き換え、後ろにスペースを 1 つ足して桁を揃え、`//COMBINED DD DISP=SHR,DSN=&SYSUID..OUTPUT(NAMES)` にします。あとは Ctrl+S で保存し、`JCL2` を右クリックして Submit Job を選ぶだけです。今度は CC 0000 になり、`OUTPUT` データセットの正しいメンバーに正しい出力が入りました。

ここから得た一般則は次の通りです。z/OS プログラムがデータセットと正しく動作するには、4 つが揃う必要があります。プログラム内部のファイル定義、`DSN=` で指定する実際のデータセット、正しい `DISP=`、そしてプログラムのファイル定義と名前が一致する DD 文です。

### パート 4 — DISP、DUMMY、そして出力を完全に正しくする

9. `JCL3` を開きました。これは複数のステップで `IEBGENER` を使い、順次データセットにレコードを書き込みます。コピー元データは、DD 文の直後にイン・ストリームで記述されており、`DSN=` ではなく `DD *` を使います。内容は時刻表で、ニューヨーク州ポキプシーからニューヨークのグランド・セントラル駅までの駅名と、ヘッダーおよび営業時間のテキストです。
10. DISP をきちんと学びました。第 1 フィールドは状態で、`NEW`（新規作成）、`SHR`（再利用、共有可）、`OLD`（再利用、排他的）、`MOD`（追加、なければ新規作成、順次データセットのみ）です。第 2 フィールドは正常終了時の処理（`DELETE`、`CATLG`、`PASS`）、第 3 フィールドは異常終了時の処理です。
11. `DUMMY` を学びました。これは「この DD は必須だが、今回は実際には使わないので、設定に時間をかけなくてよい」という意味です。IEBGENER は、出力が不要な場合でも `SYSIN` と `SYSPRINT` の DD を要求します。
12. `JCL3` をサブミットすると CC 0000 になりました。しかし、CC 0000 は「壊れていない」ことを意味するだけで、出力が正しいことを意味しません。出力を見直すと、自分で入れてしまった重複が 2 か所ありました。まず `Beacon - 59mi` が 52 行目と 53 行目に続けて 2 回出ていました。直し方は、52 行目を残して 53 行目を丸ごと消すことです。48 行目の `//BEACON EXEC PGM=IEBGENER` は消してはいけません。あれはコードであって、重複したデータ行ではないからです。もう 1 か所は、`//CORTLNDT` のステップ自体が丸ごと重複していました。85 行目から始まるステップが、94 行目で一字一句同じ形で繰り返されていたのです。直し方は、最初の完全なステップ（85 行目から 92 行目）を残し、重複したブロック（94 行目から 101 行目）を全部消すことです。この 2 か所を直し、ヘッダーとフッターも正しくすれば、出力は 10 駅・重複なし、合計 23 行で `JCL3OUT` に入ります。なお、`JCL3` を再サブミットする前には、毎回 `JCL3OUT` を削除しなければなりません。
13. 最後に、`ZXP.PUBLIC.JCL` から `CHKJCL1` をサブミットし、`JCL2` と `JCL3` の両方を検証しました。CC 0000 が完了、CC 0127 は戻って再確認する必要があるという意味です。

### COBOL にとってなぜ重要か

JCL がなければ、COBOL プログラムは本番環境では動きません。コンパイル、リンク、実行そのものが JCL で記述されたジョブであり、DD 文はプログラムの FILE-CONTROL と実際のデータセットをつなぐ橋です。JCL1 で学んだデバッグの流れは、今後のすべての COBOL プログラムで使うものです。サブミットし、ジョブログを読み、1 つ修正し、再サブミットし、完了コードだけでなく出力そのものを検証する、という流れです。

次回：06 — USS1（完了するまでは概要のみ）。

---

## 中文

### JCL 一句话讲清楚

JCL 严格说不是编程语言，它是向 z/OS 描述任务的方式，让作业进入子系统（JES）去凑齐资源、排队并执行。可以把它理解成给后厨的点菜单，JES 就是看单子决定怎么做菜的后厨。在大型机上，你很少是“直接运行”一个程序，而是提交一个作业，再去读它的输出，看看到底发生了什么。

### 第一部分 — 配置与找作业

1. 提交 `ZXP.PUBLIC.JCL` 里的 `JCLSETUP`，它会分配好这个挑战需要的数据集，包括我自己的 `JCL` 数据集。里面用了符号 `&SYSUID.`，注意末尾那个句点，系统会自动把它换成我自己的用户 ID，所以所有人都能用同一份作业。跑两次会出现 DUPLICATE 数据集报错，这本身也是个教训。
2. 设好 JOBS 过滤器：所有者是自己的用户 ID，前缀是 `*`，作业 ID 留空。在里面找到 `JCLSETUP`，打开输出，在 `JESMSGLG` 里确认 CC 0000。CC 0 表示一切按预期跑完，其他数字都值得查一查。
3. 还学了个捷径：提交后 VS Code 右下角弹出的消息，点一下就能直接跳到作业输出，不用在 JOBS 视图里翻。作业一开始是 ACTIVE 状态，关掉再打开它前面的三角，状态就会刷新。

### 第二部分 — 第一个复制作业，和第一个报错

4. 把 `ZXP.PUBLIC.JCL` 里的 `JCL1` 成员复制到我自己的 `JCL` 数据集里，因为后面要改它。这个作业用 `IEBGENER` 工具把 `JCL2` 成员复制到我的数据集里。
5. 提交后拿到 CC 0012。看作业日志（`JES2:JESMSGLG`），原因是缺了 `SYSIN` 的 DD 语句。IEBGENER 需要 `SYSPRINT`（消息）、`SYSUT1`（源）、`SYSUT2`（目标）和 `SYSIN`（控制语句，没有就用 DUMMY）。把缺的 DD 补上、保存、再提交，就拿到 CC 0000，还多了一个 `JCL2` 成员（第二步还生成了 `JCL3` 成员）。

这里的关键词汇是：**JOB** 标识作业，**EXEC** 指定要运行的程序，**DD（Data Definition，数据定义）** 把程序和输入输出数据集连起来。JCL 里的 DD 名必须和程序期望的名字对上。

### 第三部分 — 编译并运行 COBOL，再修一个 ABEND

6. 打开我自己的 `JCL2`，它负责编译 COBOL 源码，并把生成的程序放进我的 `LOAD` 数据集，那里面的程序是二进制，在 VS Code 里看不到。`//COBRUN` 这一步是编译，`//RUN EXEC PGM=CBL0001` 这一步是运行程序。
7. 提交 `JCL2` 后果然出现 ABEND（异常结束），挑战里提前说了会这样。这次没有工具手册可查，COBOL 代码本身就是文档。我打开 `//COBOL.SYSIN` 指向的 COBOL 源码，看它的 **FILE-CONTROL** 部分，把程序里用的文件名和 JCL 里的 DD 名逐一对照，例如程序里的 `FIRST-NAME` 文件对应 JCL 里的 `FNAMES` DD 语句。
8. 原因就差一个字母。程序要的是 `COMBINED`，我 `JCL2` 第 27 行写的却是 `//COMBINE DD DISP=SHR,DSN=&SYSUID..OUTPUT(NAMES)`，编辑器里标着“FIX THIS ONE”的就是这一行。改法很简单：改成 `//COMBINED`，后面多补一个空格把格式对齐，变成 `//COMBINED DD DISP=SHR,DSN=&SYSUID..OUTPUT(NAMES)`。然后 Ctrl+S 保存，右键点 `JCL2`，选 Submit Job，这次就拿到 CC 0000，正确的输出也进了 `OUTPUT` 数据集里正确的成员。

由此得出的通用规则是：一个 z/OS 程序要正常用数据集，四样东西必须对齐——程序内部的文件定义、`DSN=` 里写的实际数据集、正确的 `DISP=`，以及名字和程序文件定义一致的 DD 语句。

### 第四部分 — DISP、DUMMY，以及把输出做到完全正确

9. 打开 `JCL3`，它用好几个步骤的 `IEBGENER` 往一个顺序数据集里写记录，源数据是“流内数据”，写在 DD 语句后面，用 `DD *` 而不是 `DSN=`。内容是一张时刻表：从纽约州波基普西到纽约中央车站的各站站名，加上开头和营业时间说明。
10. 认真学了 DISP：第一个字段是状态，`NEW`（新建）、`SHR`（复用、可共享）、`OLD`（复用、独占）、`MOD`（追加，没有就新建，仅顺序数据集）。第二个字段是正常结束时怎么处理（`DELETE`、`CATLG`、`PASS`），第三个字段是失败时怎么处理。
11. 学了 `DUMMY`：意思是“这个 DD 是必需的，但这次实际不会用，别花时间去配它”。即使输出不需要，IEBGENER 也还是要求有 `SYSIN` 和 `SYSPRINT` 的 DD。
12. 提交 `JCL3` 拿到 CC 0000，但 CC 0000 只表示“没坏”，不表示输出是对的。对着输出一查，我自己写重的有两处。第一处，`Beacon - 59mi` 在第 52 行和第 53 行连着出现了两次。改法是：留第 52 行，把第 53 行整行删掉。注意别把第 48 行的 `//BEACON EXEC PGM=IEBGENER` 删了，那是代码，不是写重的数据行。第二处，`//CORTLNDT` 这个步骤整个写重了：第 85 行开始的步骤，到第 94 行又原样重复了一遍。改法是：留前面完整的一步（第 85 行到第 92 行），把后面重复的那一整块（第 94 行到第 101 行）全删掉。这两处改完，开头结尾再对好，输出就是 10 个站、没有重复，总共 23 行写进 `JCL3OUT`。而且每次重交 `JCL3` 之前，都要先把 `JCL3OUT` 删掉。
13. 最后提交 `ZXP.PUBLIC.JCL` 里的 `CHKJCL1`，同时验证 `JCL2` 和 `JCL3`，CC 0000 是完成，CC 0127 是回去再检查。

### 对以后学 COBOL 有什么用

没有 JCL，COBOL 程序在生产环境里跑不起来。编译、连接、运行本身就是用 JCL 描述的作业，而 DD 语句是程序的 FILE-CONTROL 和真实数据集之间的桥。JCL1 教我的调试循环，以后每个 COBOL 程序都会用上：提交、读作业日志、改一处、再提交、验证输出本身，而不只是看完成码。

下一篇：06 — USS1（做完之前只有大纲）。