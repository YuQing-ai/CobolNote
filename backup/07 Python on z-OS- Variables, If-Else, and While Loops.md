# 07 Python on z/OS: Variables, If/Else, and While Loops

> Series: IBM Z Xplore — Fundamentals + Concepts | Challenge: CODE1 | Beginner, Fundamentals | 120 minutes
>
> Platform status: Completed as of 2026-11-03. CHKCODE submitted and passed; the platform showed "Challenge Complete!" and recorded the progress.
>
> Prerequisite: VSC1. Source: IBM Z Xplore official CODE1 challenge documentation, plus my own practice.
>
> Language order in this article: English → 日本語 → 中文. The title stays in English.

---

## English

### What this challenge is, in one paragraph

CODE1 is not a COBOL challenge. It is a first look at programming, done in Python on z/OS, and its point is that the platform does not change the ideas: variables holding values, conditions deciding what runs, loops repeating a block, and functions packaging logic. Python is used because it is a general-purpose language that is easy to start with, and because once the fundamentals are clear, learning another language is mostly learning its particular syntax. For a COBOL programmer, the payoff is reading code rather than just running it: being able to look at a program, follow its logic, and fix it when it does not do what it should.

### Part 1 — Running code1.py, and reading it

1. I ran `python3 code1.py` in my USS terminal. The output started with "Welcome to the CODE challenge!", then printed the current time once a second, and finished with a line about my userid. The line said, in effect, that if you add up all the numbers in your userid, you get 17, which is exactly right for z70127: 7 + 0 + 1 + 2 + 7 = 17. The tutorial example showed 40, because its example userid was z99994. That difference is not an error; it is the program computing something from whoever is running it.

2. Opening code1.py in VS Code showed why. The green `#` lines are comments, not code. The program imports `datetime` to tell the time, `time` to use `sleep`, and `getpass` to get the userid, and it prints with `\n` where it wants an extra newline. Reading the code after running it, rather than before, made the output make sense: the time lines, the userid line, and the one-second pauses were all there in the source.

### Part 2 — A variable, and a word that does nothing

3. code2.py is a small investigation: does the letter `z` appear in a word? Line 6 sets `the_letter = "z"`, and line 7 sets `the_word = "pumpkin"`. A variable is a placeholder for a value, like saying x = 5 in algebra, except here it holds a word and a letter. The advantage of using variables, as the comments in the file point out, is that you do not have to rewrite the checking code when you want to try a different word or letter; you just change the value.

4. Running `python3 code2.py` with `pumpkin` produced nothing, which is the correct behaviour for a word without a `z` when there is no `else` branch yet. I changed line 7 to `the_word = "pizza"`, saved the file, and ran it again in the USS terminal. This time it printed: "Yes! The letter z is in the word pizza". That is the whole lesson of this step in miniature: change one value, save, rerun, and read the output. If you forget to save, you are testing the old file, not the new one.

<img width="1414" height="1141" alt="Image" src="https://github.com/user-attachments/assets/969d286a-fdac-4409-ba9e-b5d5931f4337" />


### Part 3 — Else, tested both ways

5. Lines 15 and 16 of code2.py were commented out: `#else:` and `# print("Nope. The letter " + the_letter + " is NOT in the word " + the_word)`. I deleted the `#` at the start of both lines, so the `else` lined up with the `if` on line 13, and its print line stayed indented under it. Indentation is not decoration in Python; it is how the program knows which lines belong to which branch.

6. Then I tested both branches, not just the happy one. With `pizza`, the output was still "Yes! The letter z is in the word pizza". I changed the word to `"apple"` and ran it again, and this time it printed: "Nope. The letter z is NOT in the word apple". A branch you have only tested one way is a branch you have not really tested.

<img width="925" height="976" alt="Image" src="https://github.com/user-attachments/assets/4da6bc2b-3025-46d2-acea-a1db0d81d53c" />


### Part 4 — Marbles, and a loop that prints one line ten times

7. marbles.py starts with `marbles = 10` and `marble_dots = "**********"`, ten asterisks pretending to be ten marbles. The `while (marbles > 0):` loop on line 6 says: keep doing this block as long as there are more than zero marbles left. Inside the loop it prints "You have " + str(marbles) + " marbles left.", where `str(marbles)` is needed because `marbles` is a number and the rest of the message is text. The message appears only once in the code but prints ten times, once per loop, because the loop repeats it and `marbles -= 1` at the end of each pass reduces the count by one.

8. The warning bug was the clearest problem-solving moment in this challenge. Lines 13 and 14, once uncommented, read `if (marbles > 3):` followed by `print("Warning: You are running low on marbles!!")`. Run like that, the warning appeared after "You have 5 marbles left." and "You have 4 marbles left.", and did not appear after 3, 2, or 1. That is exactly backwards: the condition was testing for plenty of marbles, not few. The fix was to change the condition to `if (marbles <= 3):`, less than or equal to three. I did not move the `if` or rewrite the loop; changing one comparison operator was enough, because the `if` was already in the right place, after the "You have" print and before the subtraction, so it checked the count that had just been reported.

<img width="940" height="1021" alt="Image" src="https://github.com/user-attachments/assets/3c6e7098-e217-4c06-8a0d-161c5bf2e514" />


### Part 5 — Drawing the marbles, and finishing cleanly

9. The visual step was to add one line inside the loop, before the "You have" print: `print(marble_dots[:marbles])`. The `[:marbles]` slice takes only the first `marbles` characters of the ten-asterisk string, so when 10 are left it prints ten asterisks, when 7 are left it prints seven, and so on. After the fix, the output ran down as expected: ********** with 10 left, ********* with 9, ******** with 8, ******* with 7, ****** with 6, and so on, with the warning only from 3 downwards.

<img width="911" height="1065" alt="Image" src="https://github.com/user-attachments/assets/96fbf7c4-dbfd-47d0-84c5-ae6de8094a35" />


10. The last message had to appear once, and last. In the final file, `print("You are all out of marbles")` sits at line 25 with no indentation, outside and after the `while` loop, so it runs only when the loop has finished, when `marbles` has reached zero. The final output confirmed it: *** with 3 left and the warning, ** with 2 left and the warning, * with 1 left and the warning, then a blank line, then "You are all out of marbles", and nothing after it. A message indented inside the loop would have printed ten times; a message with no indentation prints once, at the end, because that is where it belongs.

<img width="916" height="1040" alt="Image" src="https://github.com/user-attachments/assets/94798f91-7316-4121-ac98-a4d3c967d15d" />


### Part 6 — Validation

11. Finally, I submitted the CHKCODE job from ZXP.PUBLIC.JCL, the same way as the earlier checks: find CHKCODE in the dataset list and submit it. The validation job runs my marbles.py, so it checks the whole thing at once: the right number of asterisks, the right spacing, the warning only from three downwards, and the final message exactly as in the tutorial screenshot. It passed, and the platform showed "Challenge Complete!" with the note "Great work — your progress has been recorded." That is the point where this challenge stopped being a program I had run locally and became a completed step in Fundamentals.

<img width="1290" height="1005" alt="Image" src="https://github.com/user-attachments/assets/594b7c82-a3a8-4e0d-838c-859a789f3c21" />


### Why this matters for COBOL

A COBOL program is also variables, conditions, loops, and subroutines, just written in a different syntax and run against datasets instead of files in a home directory. Learning to read Python first, on the same z/OS system, means the next time a COBOL program does not do what it should, I have a method: run it, read the actual output, find the one condition or the one line that is wrong, change it, save, and rerun. The two fixes in this challenge both rewarded that method. The `else` branch was not tested until I tried a word without a `z`, and the warning was not right until I read which counts it actually fired on, rather than which counts I assumed it would fire on.

Next: Wrap Up — wrapping up Fundamentals.

---

## 日本語

### このチャレンジとは、一言でいうと

CODE1 は COBOL のチャレンジではありません。z/OS 上で Python を使ったプログラミングの入門で、要点は、プラットフォームが変わっても考え方は変わらないということです。値を保持する変数、実行する内容を決める条件、ブロックを繰り返すループ、ロジックをまとめる関数です。Python が使われるのは、汎用言語で始めやすく、基礎が分かれば、別の言語を学ぶことは主にその言語特有の構文を学ぶことになるからです。COBOL のプログラマーにとっての収穫は、プログラムを実行するだけでなく、読むことです。プログラムを見て、ロジックを追い、思った通りに動かないときに直せることです。

### パート 1 — code1.py を実行し、読む

1. USS ターミナルで `python3 code1.py` を実行しました。出力は "Welcome to the CODE challenge!" から始まり、現在時刻を 1 秒ごとに表示し、最後に自分のユーザーIDについての行で終わりました。その行は、ユーザーID の数字を全部足すと 17 になるという内容で、z70127 なら 7 + 0 + 1 + 2 + 7 = 17 で、まさにその通りです。チュートリアルの例は 40 でしたが、それは例のユーザーID が z99994 だったからです。この違いはエラーではなく、実行した人から計算しているプログラムの当然の結果です。

2. code1.py を VS Code で開くと、理由が分かりました。緑色の `#` の行はコメントで、コードではありません。プログラムは、時刻を知るために `datetime`、スリープのために `time`、ユーザーID を取得するために `getpass` をインポートし、改行を入れたい場所で `\n` を使って出力します。実行してから読むことで、出力の意味が分かりました。時刻の行、ユーザーID の行、1 秒の間隔は、すべてソースの中にありました。

### パート 2 — 変数と、何も起こらない単語

3. code2.py は小さな調査です。文字 `z` は単語の中にあるか。第 6 行は `the_letter = "z"`、第 7 行は `the_word = "pumpkin"` です。変数は値の入れ物で、代数で x = 5 と言うようなものですが、ここでは単語と文字を保持します。ファイルのコメントにもある通り、変数を使う利点は、別の単語や文字を試したいときに、チェックするコードを書き直す必要がなく、値を変えるだけで済むことです。

4. `pumpkin` で `python3 code2.py` を実行すると、何も出ませんでした。`else` がまだない状態で、z のない単語なら、それが正しい動作です。第 7 行を `the_word = "pizza"` に変え、ファイルを保存し、USS ターミナルで再実行すると、今度は "Yes! The letter z is in the word pizza" と出ました。このステップの教訓は、ミニチュア版で言えばこれだけです。値を 1 つ変え、保存し、再実行し、出力を読む。保存を忘れると、新しいファイルではなく、古いファイルをテストしていることになります。

### パート 3 — else を、両方向でテストする

5. code2.py の第 15、16 行はコメントアウトされていました。`#else:` と `# print("Nope. The letter " + the_letter + " is NOT in the word " + the_word)` です。両行の先頭の `#` を削除し、`else` が第 13 行の `if` と揃い、その下の print 行がインデントされたままになるようにしました。Python では、インデントは飾りではありません。どの行がどの分岐に属するかを、プログラムが知る方法です。

6. 次に、うまくいく場合だけでなく、両方の分岐をテストしました。`pizza` では、出力は変わらず "Yes! The letter z is in the word pizza" でした。単語を `"apple"` に変えて再実行すると、今度は "Nope. The letter z is NOT in the word apple" と出ました。片方向しかテストしていない分岐は、実はテストしていないのと同じです。

### パート 4 — ビー玉と、1 行を 10 回表示するループ

7. marbles.py は `marbles = 10` と `marble_dots = "**********"`、つまり 10 個のアスタリスクを 10 個のビー玉に見立てたものから始まります。第 6 行の `while (marbles > 0):` ループは、ビー玉が 0 より多い間、このブロックを続けなさいという意味です。ループの中では "You have " + str(marbles) + " marbles left." と表示します。`str(marbles)` が必要なのは、`marbles` が数字で、メッセージの残りが文字列だからです。このメッセージはコードの中には 1 回しか出てきませんが、10 回表示されます。ループが繰り返し、最後の `marbles -= 1` が毎回 1 ずつ減らすからです。

8. 警告のバグは、このチャレンジで最もはっきりした問題解決の場面でした。コメントを外した第 13、14 行は `if (marbles > 3):` の後に `print("Warning: You are running low on marbles!!")` というものでした。このまま実行すると、警告は "You have 5 marbles left." と "You have 4 marbles left." の後に出て、3、2、1 の後には出ませんでした。まさに逆です。条件が、ビー玉が少ない場合ではなく、多い場合をテストしていたからです。修正は、条件を `if (marbles <= 3):`、つまり 3 以下に変えることでした。`if` を動かしたり、ループを書き直したりはしませんでした。比較演算子を 1 つ変えるだけで十分でした。`if` はすでに正しい場所にあり、「You have」の表示の後、引き算の前にあって、ちょうど報告された数をチェックしていたからです。

### パート 5 — ビー玉を描き、きれいに終わる

9. 視覚化のステップは、ループの中、「You have」の表示の前に、1 行を追加することでした。`print(marble_dots[:marbles])` です。`[:marbles]` というスライスは、10 個のアスタリスクの文字列から、最初の `marbles` 個の文字だけを取ります。10 個残っていれば 10 個、7 個残っていれば 7 個表示されます。修正後、出力は予想通りになりました。10 個残りで **********、9 個で *********、8 個で ********、7 個で *******、6 個で ******、という具合で、警告は 3 から下だけに出ました。

10. 最後のメッセージは、1 回だけ、最後に出なければなりませんでした。最終的なファイルでは、`print("You are all out of marbles")` は第 25 行にインデントなしで置かれ、ループの外、後ろにあります。だから、ループが終わり、`marbles` が 0 になったときにだけ実行されます。最終的な出力がそれを確認しました。3 個残りで *** と警告、2 個残りで ** と警告、1 個残りで * と警告、その後、空行、そして "You are all out of marbles"、それ以降は何もありません。ループの中にインデントされたメッセージなら 10 回表示されます。インデントなしのメッセージは、最後に 1 回だけ表示されます。それが、そのメッセージのあるべき場所だからです。

### パート 6 — 検証

11. 最後に、ZXP.PUBLIC.JCL から CHKCODE ジョブをサブミットしました。やり方はこれまでのチェックと同じで、データセット一覧から CHKCODE を見つけてサブミットします。検証ジョブは自分の marbles.py を実行するので、アスタリスクの数、間隔、警告が 3 から下だけに出ること、最後のメッセージがチュートリアルのスクリーンショット通りであることを、一度にチェックします。合格し、プラットフォームには "Challenge Complete!" と "Great work — your progress has been recorded." と表示されました。ここで、このチャレンジは、自分で実行したプログラムから、Fundamentals の完了したステップになりました。

### COBOL にとってなぜ重要か

COBOL のプログラムも、変数、条件、ループ、サブルーチンでできています。構文が違い、ホームディレクトリのファイルではなく、データセットに対して実行されるだけです。同じ z/OS システム上で、まず Python を読めるようになることは、次に COBOL のプログラムが思った通りに動かないときに、方法を持っているということです。実行し、実際の出力を読み、間違っている条件や行を 1 つ見つけ、変え、保存し、再実行する。このチャレンジの 2 つの修正は、どちらもその方法を報いるものでした。`else` の分岐は、z のない単語を試すまでテストされず、警告は、自分がそうなると思っていた数ではなく、実際にどの数で出たかを読むまで、正しくなりませんでした。

次回：Wrap Up — Fundamentals のまとめ。

---

## 中文

### 这一关一句话讲清楚

CODE1 不是 COBOL 关，而是用 Python 在 z/OS 上讲编程入门，重点是平台换了，想法不变：变量存值、条件决定跑什么、循环重复一个块、函数把逻辑包起来。选 Python 是因为它是通用语言，容易上手，而且基本功通了，再学别的语言主要就是学它的特别语法。对 COBOL 程序员来说，收益在读代码，而不只是跑代码：能看一段程序、跟上它的逻辑、它不对的时候能修。

### 第一部分 — 跑 code1.py，再读它

1. 我在 USS 终端里跑了 `python3 code1.py`。输出先是 Welcome to the CODE challenge!，然后每秒打印一次当前时间，最后是一句关于用户 ID 的话，大意是把用户 ID 里的数字全加起来是 17，对 z70127 来说 7 + 0 + 1 + 2 + 7 正好等于 17。教程示例是 40，因为示例用户是 z99994，这个差别不是错，是程序在按跑它的人算。

2. 在 VS Code 里打开 code1.py 就明白了。绿色的 # 开头是注释，不是代码。程序导入 datetime 是为了知道时间，time 是为了用 sleep，getpass 是为了拿到用户 ID，打印时用 `\n` 表示多一个换行。先跑再读，输出就对上号了：时间那几行、用户 ID 那句、每段之间停一秒，都在源码里。

### 第二部分 — 变量，和一个什么都不发生的词

3. code2.py 是个小调查：字母 z 在不在一个词里？第 6 行是 `the_letter = "z"`，第 7 行是 `the_word = "pumpkin"`。变量就是值的占位符，跟代数里说 x = 5 一个意思，只不过这里装的是词和字母。文件注释里也说了用变量的好处：想试别的词或字母，不用重写检查的代码，改值就行。

4. 用 pumpkin 跑 `python3 code2.py`，什么都没出，在还没有 else 分支的时候，对一个没有 z 的词来说，这就是正确的表现。我把第 7 行改成 `the_word = "pizza"`，保存，再在 USS 终端里跑一次，这次出了 Yes! The letter z is in the word pizza。这一步的教训缩到最小就是：改一个值、保存、重跑、读输出。忘了保存，你测的就是旧文件，不是新文件。

### 第三部分 — else，要两边都测

5. code2.py 第 15、16 行原来是注释掉的：`#else:` 和 `# print("Nope. The letter " + the_letter + " is NOT in the word " + the_word)`。我把两行开头的 # 删掉，else 跟第 13 行的 if 对齐，它下面的 print 行保持缩进。在 Python 里缩进不是装饰，是程序判断哪行属于哪个分支的办法。

6. 然后我两边都测了，不只测顺的那边。pizza 还是 Yes! The letter z is in the word pizza。把词改成 "apple" 再跑，这次出了 Nope. The letter z is NOT in the word apple。只测过一个方向的分支，等于没真正测过。

### 第四部分 — 弹珠，和一行打印十次的循环

7. marbles.py 开头是 `marbles = 10` 和 `marble_dots = "**********"`，十个星号假装是十颗弹珠。第 6 行的 `while (marbles > 0):` 循环意思是：只要弹珠数大于 0，就一直做这个块。块里打印 "You have " + str(marbles) + " marbles left."，其中 str(marbles) 是必须的，因为 marbles 是数字，消息其余部分是文字。这句话在代码里只出现一次，却打印了十次，因为循环在重复它，每轮末尾的 `marbles -= 1` 又把数减一。

8. 警告的 bug 是这一关最清楚的一次问题解决。取消注释后的第 13、14 行是 `if (marbles > 3):` 后面跟 `print("Warning: You are running low on marbles!!")`。这样跑，警告出现在 You have 5 marbles left. 和 You have 4 marbles left. 后面，3、2、1 后面反而没有，正好反了：条件测的是弹珠多，不是弹珠少。修法是把条件改成 `if (marbles <= 3):`，小于等于三。我没挪 if，也没重写循环，只改了一个比较运算符就够了，因为 if 本来就在对的位置上，在 You have 打印之后、减一之前，正好检查刚报出来的那个数。

### 第五部分 — 把弹珠画出来，收尾收干净

9. 可视化那步是在循环里、You have 打印之前加一行：`print(marble_dots[:marbles])`。`[:marbles]` 这种切片是只取十个星号字符串的前 marbles 个字符，剩 10 颗就打印十个星号，剩 7 颗就打印七个。改完后输出就跟预期一样了：10 颗是 **********，9 颗是 *********，8 颗是 ********，7 颗是 *******，6 颗是 ******，往下走，警告只从 3 开始出现。

10. 最后一句话必须只出现一次，而且出现在最后。最终文件里，`print("You are all out of marbles")` 在第 25 行，没有缩进，在 while 循环外面、后面，所以只有循环跑完、marbles 到 0 的时候才执行。最终输出也证实了这一点：3 颗是 *** 加警告，2 颗是 ** 加警告，1 颗是 * 加警告，然后空一行，再是 You are all out of marbles，后面什么都没有。缩进在循环里的消息会打印十次，没有缩进的消息只在最后打印一次，因为那就是它该在的位置。

### 第六部分 — 验证

11. 最后，我从 ZXP.PUBLIC.JCL 提交了 CHKCODE 作业，办法跟前面几次检查一样，在数据集列表里找到 CHKCODE 提交。验证作业跑的是我的 marbles.py，所以一次就全查了：星号数对不对、空格对不对、警告是不是只从 3 开始出现、最后一句话是不是跟教程截图一字不差。通过了，平台弹出 Challenge Complete!，写着 Great work — your progress has been recorded。到这一步，这一关才从“我自己跑过的程序”变成 Fundamentals 里一个真正完成的一步。

### 对以后学 COBOL 有什么用

COBOL 程序也是变量、条件、循环、子程序，只是语法不同，跑的对象从主目录里的文件换成了数据集。在同一台 z/OS 上先学会读 Python，意味着下次 COBOL 程序不对的时候，我有办法：跑它、读真实输出、找到错的那一个条件或那一行、改掉、保存、重跑。这一关的两个修法都奖励了这个办法。else 分支是不试一个没有 z 的词就测不出来，警告是不读它实际在哪些数上触发、只凭自己以为的数就修不对。

下一篇：Wrap Up — Fundamentals 收尾。
