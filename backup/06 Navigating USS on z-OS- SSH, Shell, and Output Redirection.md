> Series: IBM Z Xplore — Fundamentals + Concepts | Challenge: USS1 | Beginner, Fundamentals | 60 minutes
>
> Platform status: Completed (as of 2026-11-03)
>
> Prerequisite: VSC1, FILES1, JCL1. Source: IBM Z Xplore official USS1 challenge documentation, plus my own practice.
>
> Language order in this article: English → 日本語 → 中文. The title stays in English.

---

## English

### What USS is, in one paragraph

USS, UNIX System Services, is a POSIX-compliant UNIX environment inside z/OS. It is not a separate machine. It runs on the same system APIs as the z/OS I had already been using, but it organizes things the UNIX way: directories inside directories, files inside directories. That sits alongside what I learned in FILES1, where data lives in datasets and members. A COBOL programmer ends up needing both, because source and business data often live in datasets, while scripts and modern tooling often live in USS.

### Part 1 — Getting into the shell

1. I opened a terminal in VS Code and switched from PowerShell to CMD first, because the tutorial warns that ssh in PowerShell can fail with Permission denied even when the password is right. I typed `cmd` at the PowerShell prompt, then `ssh z70127@204.90.115.200`.

2. The first password attempt came back with Permission denied, please try again. I typed the password again and got in. Nothing in the tutorial was wrong here; it was just a failed first attempt, and retyping it solved it.

3. After login, the banner said IBM Z Xplore learning platform, UNIX System Services 3.1, with the usual warnings: do not install my own copies of the centrally installed tools, do not create virtual environments, do not install products without authorization, and do not upload commercial source code. One line also appeared before the prompt: `/etc/zprofile:338: FSUZ0155 no matches found: /z/z70127/.npm/_logs/*.log`. It did not block anything, so I left it alone and carried on.

4. My first `ls` showed only `tmp`. I ran `uss-setup`, got back "Hi and welcome to the ZXPLORE UNIX System Service Challenges!", and ran `ls` again. This time the home directory was full, and it was richer than the tutorial example: `animals.sh`, `assembler`, `cobol-check.config.properties`, `codeathons`, `directory1`, `dslist.py`, `movie-data.csv`, `members.py`, `odbc`, and more, left over from earlier work. That was a useful reminder that a real home directory is not a clean textbook example.

<img width="910" height="970" alt="Image" src="https://github.com/user-attachments/assets/bc83f9b2-2c3e-4912-9424-f1737a46bc47" />


### Part 2 — Moving around, and making files

5. I checked where I was with `pwd`, which printed `/z/z70127`. Then `cd ~` to go home, `cd directory1` to go in, `pwd` again to confirm `/z/z70127/directory1`, and `cd ..` to go back one level.

6. Then I made something of my own: `touch mynewfile` and `mkdir mynewdir`, and verified both with `ls`. The listing now showed `mynewdir` and `mynewfile` alongside everything else, including `code1.py`, `code2.py`, `go2`, `hackathons`, `j2p`, `scramble.sh`, `secret.txt`, `test`, and `zosmf`. I did not delete them at this point, and later the `du` output still showed them at 0, which is exactly what an empty file and an empty directory should look like.

<img width="875" height="441" alt="Image" src="https://github.com/user-attachments/assets/5d60b587-96f2-4419-b584-400a2041398c" />


### Part 3 — The same files in Zowe

7. A shell is one way in, Zowe is another. In the Zowe Explorer USS view I set the filter to `/z/z70127`, my own home directory in all lowercase. Seeing the same home directory in the terminal and in Zowe made the point concrete: USS files are not locked inside the terminal, and datasets are not the only way to look at what is on the system. I chose the terminal for the rest of this challenge and Zowe for checking, because typing the commands is what I needed to learn, and the view is what I needed to confirm.

<img width="1106" height="455" alt="Image" src="https://github.com/user-attachments/assets/a9853beb-2d41-4358-83df-347a5ed5de6a" />


### Part 4 — Cracking the secret

8. The task was to run `scramble.sh` against `/z/public/secret.txt` and find the rotation that decodes it. The program takes a file as its first argument and a rotation number as its second, and it only works on lowercase letters. It prints the decoded output, or garbage if the number is wrong.

9. I tried in this order: 13, 7, 3, and 12. Every one of them produced a line of unreadable text, each starting with "Processing..........Done!" and then garbage. The fifth try, 17, produced: "congratulations! you unscrambled the message in the uss challenge!!" That was the whole method, and it is worth stating plainly: there was no shortcut, just five trials, four wrong answers discarded, and one right answer kept. Trying a number, reading the output, and deciding "not this one" is the work, not a failure before the work.

<img width="871" height="824" alt="Image" src="https://github.com/user-attachments/assets/79b0aced-e8ae-4e12-a81c-1ae39bb71b60" />


### Part 5 — Redirection, and getting the file exactly right

10. Redirection is where `>` and `>>` stop being theory. I ran the successful command again with `> ussout.txt` at the end, so the output went into the file instead of onto the screen, and verified it with `cat ussout.txt`. Then I appended, never overwrote, with `>>`: first `du -ak >> ussout.txt`, then `date >> ussout.txt`. The difference matters, and this challenge is a good place to learn it, because overwriting at step 11 or 12 would have wiped out the decoded message.

<img width="885" height="975" alt="Image" src="https://github.com/user-attachments/assets/53c81358-653a-4712-a3d4-c63509d5ff80" />

11. `du -ak` lists disk usage in kilobytes, with `-a` covering the current directory and everything below it. My home directory came out at 694 in total, and the listing included the leftovers from earlier challenges, the 0-size `mynewfile` and `mynewdir` I had just made, and `ussout.txt` itself at 8. The final `cat ussout.txt` showed the file in the right order: first the decoded message, then the `du` output, then the date, Tue Nov 3 00:37:34 CST 2026.

<img width="794" height="796" alt="Image" src="https://github.com/user-attachments/assets/0001afac-1f1d-4203-81dd-6d8a3e480f01" />



### Part 6 — Validation

12. I submitted the validation job `CHKUSS1` from `ZXP.PUBLIC.JCL`. In the Jobs view it showed as `@USS1(JOB02768) - SOW1 - CC 0000`. Then the platform showed the Challenge Complete overlay: "Great work — your progress has been recorded." That is the evidence, not just my word for it.

<img width="595" height="999" alt="Image" src="https://github.com/user-attachments/assets/2176b589-f641-4779-9b1e-76807d4adfef" />

<img width="1294" height="1004" alt="Image" src="https://github.com/user-attachments/assets/73b7a8b7-0ebb-4856-8c19-fc57435ce58b" />



### Why this matters for COBOL

Without USS, a COBOL programmer is stuck in one half of the system. Datasets hold the source and the data, but the shell is where you move files around, run scripts, check disk usage, and chain commands together with redirection. The next challenge, Code, stays in USS and uses Python there, so this is not a detour. It is the working environment for what comes next. What I took away most is not any single command, but the habit: run a command, read its actual output, and only then decide what to do next. The scramble trials and the `>` versus `>>` distinction both rewarded exactly that habit.

Next: Code — learn the basics of programming using Python directly on a z/OS mainframe system (120 minutes).

---

## 日本語

### USS とは、一言でいうと

USS（UNIX System Services）は、z/OS の中にある POSIX 準拠の UNIX 環境です。別のマシンではありません。すでに使ってきた z/OS と同じシステム API の上で動きますが、整理の仕方は UNIX 流で、ディレクトリの中にディレクトリ、ディレクトリの中にファイルがあります。FILES1 で学んだデータセットとメンバーという整理の仕方と並んで存在します。COBOL のプログラマーには両方が必要です。ソースや業務データはデータセットにあることが多く、スクリプトやモダンなツールは USS にあることが多いからです。

### パート 1 — シェルに入る

1. VS Code でターミナルを開き、まず PowerShell から CMD に切り替えました。チュートリアルに、PowerShell で ssh を使うとパスワードが合っていても Permission denied になることがあると書いてあったからです。PowerShell のプロンプトで `cmd` と打ち、次に `ssh z70127@204.90.115.200` と打ちました。

2. 最初のパスワード入力は Permission denied, please try again. と返ってきました。もう一度入力したら入ることができました。チュートリアルが間違っていたわけではなく、最初の入力が失敗しただけで、打ち直したら解決しました。

3. ログイン後のバナーには IBM Z Xplore learning platform、UNIX System Services 3.1 と表示され、集中管理されているツールは自分で入れ直さないこと、仮想環境を作らないこと、許可なく製品を入れないこと、商用のソースコードをアップロードしないこと、といった注意書きがありました。プロンプトの前には `/etc/zprofile:338: FSUZ0155 no matches found: /z/z70127/.npm/_logs/*.log` という行も出ましたが、何も妨げなかったので、そのまま進めました。

4. 最初の `ls` では `tmp` しか表示されませんでした。`uss-setup` を実行すると "Hi and welcome to the ZXPLORE UNIX System Service Challenges!" と返ってきて、もう一度 `ls` を実行すると、ホームディレクトリはチュートリアルの例よりもずっとにぎやかでした。`animals.sh`、`assembler`、`cobol-check.config.properties`、`codeathons`、`directory1`、`dslist.py`、`movie-data.csv`、`members.py`、`odbc` など、これまでの作業で残ったものが並んでいました。実際のホームディレクトリは教科書のきれいな例ではない、という当たり前のことを思い出させられました。

### パート 2 — 移動と、ファイルの作成

5. `pwd` で自分の位置を確認すると `/z/z70127` でした。`cd ~` でホームに戻り、`cd directory1` で入り、`pwd` で `/z/z70127/directory1` を確認し、`cd ..` で 1 つ上に戻りました。

6. 次に自分のものを作りました。`touch mynewfile` と `mkdir mynewdir` を実行し、`ls` で確認しました。一覧には `mynewdir` と `mynewfile` が加わり、そのほか `code1.py`、`code2.py`、`go2`、`hackathons`、`j2p`、`scramble.sh`、`secret.txt`、`test`、`zosmf` なども並んでいました。この時点では削除せず、後の `du` の出力でも両方とも 0 と表示されました。空のファイルと空のディレクトリなら、当然の結果です。

### パート 3 — Zowe でも同じファイルを見る

7. シェルは入口の 1 つで、Zowe はもう 1 つの入口です。Zowe Explorer の USS ビューで、フィルターを自分のホームディレクトリ `/z/z70127`（すべて小文字）に設定しました。ターミナルと Zowe で同じホームディレクトリが見えることが、ここでの要点でした。USS のファイルはターミナルの中に閉じているわけではなく、データセットだけがシステムの中身を見る方法でもありません。このチャレンジの残りは、学ぶべきはコマンドを打つこと、確認に使うのはビューだという理由で、ターミナルで進め、Zowe で確認しました。

### パート 4 — 秘密の解読

8. 課題は、`/z/public/secret.txt` に対して `scramble.sh` を実行し、解読できる回転数を見つけることでした。このプログラムは第 1 引数にファイル、第 2 引数に回転数をとり、小文字にしか効きません。数が合えば解読した出力が、合わなければ意味不明な文字列が出ます。

9. 試した順番は 13、7、3、12 でした。どれも "Processing..........Done!" の後に読めない文字列が出るだけでした。5 回目の 17 で、"congratulations! you unscrambled the message in the uss challenge!!" と出ました。方法はこれだけです。近道はなく、4 回の間違いを捨て、1 回の正解を残しただけです。数を試し、出力を読み、「これではない」と判断すること自体が作業であって、作業の前の失敗ではありません。

### パート 5 — リダイレクトと、ファイルをきちんと作ること

10. リダイレクトは、`>` と `>>` が理論でなくなる場所です。成功したコマンドをもう一度実行し、末尾に `> ussout.txt` を付けて、出力を画面ではなくファイルに入れ、`cat ussout.txt` で確認しました。その後は上書きせず、追加だけをしました。まず `du -ak >> ussout.txt`、次に `date >> ussout.txt` です。この違いは重要で、ステップ 11 や 12 で上書きしてしまったら、解読したメッセージが消えてしまいます。

11. `du -ak` はキロバイト単位のディスク使用量を、カレントディレクトリとその下すべてについて（`-a`）一覧にします。自分のホームディレクトリは合計 694 で、一覧にはこれまでのチャレンジの残り、作ったばかりの 0 サイズの `mynewfile` と `mynewdir`、そして `ussout.txt` 自体の 8 が含まれていました。最後の `cat ussout.txt` では、解読したメッセージ、次に `du` の出力、最後に日付 Tue Nov 3 00:37:34 CST 2026 という正しい順番で並んでいました。

### パート 6 — 検証

12. `ZXP.PUBLIC.JCL` から検証ジョブ `CHKUSS1` をサブミットしました。Jobs ビューには `@USS1(JOB02768) - SOW1 - CC 0000` と表示されました。次にプラットフォームに Challenge Complete のオーバーレイが出て、"Great work — your progress has been recorded." と表示されました。これが証拠で、自分の言葉だけではありません。

### COBOL にとってなぜ重要か

USS がなければ、COBOL のプログラマーはシステムの半分にしか入れません。データセットにはソースとデータがありますが、ファイルを動かし、スクリプトを実行し、ディスク使用量を確認し、リダイレクトでコマンドをつなげるのはシェルです。次のチャレンジ Code も USS の中で Python を使うので、これは寄り道ではありません。次に進むための作業環境そのものです。最も持ち帰ったのは、個別のコマンドではなく、習慣です。コマンドを実行し、実際の出力を読み、それから次に何をするか決める。スクランブルの試行も、`>` と `>>` の区別も、まさにその習慣を報いるものでした。

次回：Code — z/OS のメインフレーム上で直接 Python を使ったプログラミングの基礎（120 分）。

---

## 中文

### USS 一句话讲清楚

USS，也就是 UNIX System Services，是 z/OS 里一层符合 POSIX 标准的 UNIX 环境。它不是另一台机器，而是跑在同一套 z/OS 系统 API 上，只是按 UNIX 的方式组织东西：文件夹里套文件夹，文件夹里放文件。这和 FILES1 里学的数据集加成员是并存的两套组织方式。COBOL 程序员两边都得认，因为源码和业务数据常在数据集里，而脚本和现代工具常在 USS 里。

### 第一部分 — 进 shell

1. 我在 VS Code 里打开终端，先从 PowerShell 切到 CMD，因为教程里提醒过，PowerShell 里用 ssh 可能密码没错也报 Permission denied。我在 PowerShell 提示符下先敲 `cmd`，再敲 `ssh z70127@204.90.115.200`。

2. 第一次输密码返回了 Permission denied, please try again。再输一次就进去了。教程没错，就是第一次没输对，重输就解决了。

3. 登录后横幅写着 IBM Z Xplore learning platform、UNIX System Services 3.1，还有一串注意事项：集中安装的工具不要自己另装、不要建虚拟环境、没经过授权不要装产品、不要上传商业源码。提示符前面还出现了一行 `/etc/zprofile:338: FSUZ0155 no matches found: /z/z70127/.npm/_logs/*.log`，它没挡住任何事，我就没管它，继续往下做。

4. 第一次 `ls` 只看到 `tmp`。跑了 `uss-setup`，回了一句 Hi and welcome to the ZXPLORE UNIX System Service Challenges!，再 `ls` 一次，主目录就满了，而且比教程示例要杂得多：`animals.sh`、`assembler`、`cobol-check.config.properties`、`codeathons`、`directory1`、`dslist.py`、`movie-data.csv`、`members.py`、`odbc` 等等，都是前面做下来的残留。这倒是个实在的提醒：真实的主目录不是教科书里干干净净的例子。

### 第二部分 — 移动、建文件

5. 先用 `pwd` 看自己在哪，显示 `/z/z70127`。然后 `cd ~` 回主目录，`cd directory1` 进去，再 `pwd` 确认是 `/z/z70127/directory1`，`cd ..` 回上一级。

6. 接着建了自己的东西：`touch mynewfile` 和 `mkdir mynewdir`，用 `ls` 验证。列表里多了 `mynewdir` 和 `mynewfile`，旁边还有 `code1.py`、`code2.py`、`go2`、`hackathons`、`j2p`、`scramble.sh`、`secret.txt`、`test`、`zosmf` 等等。我这一步没删它们，后面 `du` 里它们俩都是 0，空文件和空目录就该是这个样子。

### 第三部分 — 在 Zowe 里看同一批文件

7. shell 是一种入口，Zowe 是另一种入口。我在 Zowe Explorer 的 USS 视图里把过滤器设成自己的主目录 `/z/z70127`，全小写。终端和 Zowe 里看到同一个主目录，这一点本身就是关键：USS 的文件不是只关在终端里的，数据集也不是看系统内容的唯一办法。这关剩下的部分，我用终端做、用 Zowe 核对，因为要学的是敲命令，要确认的是视图。

### 第四部分 — 解开秘密

8. 任务是对 `/z/public/secret.txt` 跑 `scramble.sh`，找出能解开它的轮转数。这个程序第一个参数是文件，第二个参数是轮转数，只对小写字母有效。数对了就输出解开的内容，数错了就输出一串乱码。

9. 我试的顺序是 13、7、3、12，每次都是先出 Processing..........Done!，然后一串读不懂的东西。第五次试 17，出了 congratulations! you unscrambled the message in the uss challenge!! 方法就是这么朴素，没有捷径，四个错的扔掉，一个对的留下。试一个数、读输出、判断“不是这个”，这本身就是干活，不是干活之前的失败。

### 第五部分 — 重定向，把文件做到完全正确

10. 重定向就是 `>` 和 `>>` 从理论变成手感的地方。我把成功的那条命令再跑一次，末尾加上 `> ussout.txt`，输出就进了文件而不是屏幕，再用 `cat ussout.txt` 验证。之后只追加、不覆盖：先 `du -ak >> ussout.txt`，再 `date >> ussout.txt`。这个区别很重要，在第 11、12 步要是手滑用了覆盖，解开的信息就没了。

11. `du -ak` 是按 KB 列磁盘用量，`-a` 是当前目录及下面所有目录都算。我的主目录合计是 694，列表里有前面各关的残留、刚建的 0 大小的 `mynewfile` 和 `mynewdir`，还有 `ussout.txt` 本身的 8。最后一次 `cat ussout.txt`，顺序正好是三段：先是解开的信息，再是 `du` 的输出，最后是日期 Tue Nov 3 00:37:34 CST 2026。

### 第六部分 — 验证

12. 我从 `ZXP.PUBLIC.JCL` 提交了验证作业 `CHKUSS1`，作业视图里显示 `@USS1(JOB02768) - SOW1 - CC 0000`。接着平台弹出了 Challenge Complete：“Great work — your progress has been recorded.” 这就是证据，不只是我自己说做完了。

### 对以后学 COBOL 有什么用

没有 USS，COBOL 程序员只能进系统的一半。数据集里有源码和数据，但搬文件、跑脚本、查磁盘用量、用重定向把命令串起来，都得靠 shell。下一关 Code 还在 USS 里用 Python，所以这不是绕路，而是接下来干活的环境本身。我带走最多的不是某一条命令，而是一个习惯：先跑命令、读真实输出，再决定下一步。解 scramble 的五次尝试和 `>` 与 `>>` 的区别，奖励的正是这个习惯。

下一篇：Code — 在 z/OS 大型机上直接用 Python 学编程基础（120 分钟）。
