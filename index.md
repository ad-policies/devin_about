---
layout: page
title: プライバシーポリシー
---

# 完全自立型 AI Devin

このセクションは[Devin AI ｜話題の世界初 "完全自律型" AI エンジニア「Devin」の公開内容の全訳](https://qiita.com/to3izo/items/9b44ec4f6a82f09a8af2)の内容を要約しています。

Cognition 社が発表した AI で、「世界初の完全自律型 ''AI ソフトウェア・エンジニア''」と表現しています。
動作デモンストレーションの中で人間から指示を受けると、問題にどう取り組むか計画を立て、人間と同じようなツールを使ってプロジェクト全体を構築したそうです。構築中にエラーが発生した場合、デバッグ用の print 分等を追加し、そのエラーの内容からバグ修正の方法を検討・実施しました。

完全自律型 AI という言葉が出てきましたが、コーディングに利用する AI の分類については大きく分けて２つ、 **支援型** と **完全自立型** が存在します。

## 支援型 AI

人間をサポートするのが主な役割で、指示やリクエストを受けてそれに基づいて動く。決定権は人間にあるので人間の作業の効率化や、判断ミスや作業負担を減らすのが目的。

## 完全自律型 AI

自分で環境を理解して、判断・行動を行う。指示の内容にもよるが、AI 自ら問題を解決する能力を持っているため人間が関与しなくても複雑なタスクを完遂する。

Devin に限って言えば、完全自律型とは言えどリアルタイムで進捗状況を報告し、それに対するユーザからのフィードバックがあれば受け入れた上で作業を続行するため人間を無視して動作するというわけではない。（その他を調べてないためこの表現にしてます）

## SWE-bench

記事の中で SWE-bench による他の AI との比較が行われていますが、そもそもそのベンチマークがどのようなものか調査しました。

GitHub 上の Python 関連のリポジトリの issue から作られた問題データセットで、2294 件の問題から構成されています。
問題の内訳については [こちら](https://blog.asial.co.jp/4646/) から拝借しています。<br>
| リポジトリ | 説明 | 問題数 |
| ---- | ----| ---- |
| astropy/astropy | 天文学用の Python ライブラリ | 95 |
| django/django | Web アプリケーションフレームワーク | 850 |
| matplotlib/matplotlib | データ可視化ライブラリ | 184 |

・<br>
・<br>
・<br>

全部は長いので省略しますが、こんな感じで様々な分野の課題に対して AI が取り組みその正答率をスコアすることで他の AI と比較しています。ちなみに対応する問題数によって３種類のベンチマークがありますが、これは 2000 問以上解くのはとんでもないコストがかかるため分けられているそうです。

## AI モデルと AI システム

[このサイト](https://www.marubeni-idigio.com/insight-hub/development-of-aisystem/#index_id3) が個人的にはわかりやすかったかなと思います。

### AI モデル

人間の知能のような「経験して学習する」思考プロセスを実現するコンピュータプログラムのこと。

### AI システム

AI モデルは人間で言うと「脳」と表現されますが、脳だけでは考えることはできてもアウトプットすることはできません。
たとえば、AI に何らかのデータを与え、AI が目的に応じた回答をアウトプットする場合、データの読み取り、AI モデルによるデータ処理、処理結果のアウトプットといった一連の仕組みが必要になります。その一連の仕組み全体が AI システムです。
※ここでのアウトプットとは AI モデルによる処理結果ではなく現実世界への干渉という意味合いだと思います。

つまり AI モデルと AI システムの関係を一言で表すと

**「AI モデルは、ある目的に沿ってインプットされたデータを処理し、何らかのアウトプットをするコンポーネントとして機能し、AI システムの一部を構成する。」**

ということになります。なので、Devin は AI システムと呼ばれる存在かと思います。

### Devin の AI モデル

Devin の脳を担当するのは同社が開発した「Devin Model」という AI モデルです。
このモデルは、コードリポジトリ、技術文書、業界の議論など幅広いソフトウェアエンジニアリング資料に対する徹底的なトレーニングの結果、複雑なプログラミング言語や概念の理解ができるようになっているそうです。

ソースは[ここ](https://note.com/ippei_suzuki_us/n/n577892d9a8cb)になりますがこのサイト以外で Devin Model という表記はされていません。しかし、公式サイトでも own ai model としか表現されていないので Devin Model を呼ぶのが良いのかなと思います。

## Devin Model のベンチマーク結果

![ベンチマーク](https://cdn.sanity.io/images/2mc9cv2v/production/2604c49c0ca7be615027add485ae9f132afccb28-1600x883.png?w=1600&fit=max&auto=format)
意図的に結果を隠しているのかまだ開発段階だからなのかわかりませんが、[Devin 公式サイト](https://www.cognition.ai/blog/swe-bench-technical-report)のこの部分が気になります。

> We evaluated Devin on a randomly chosen 25% of the SWE-benchmark test set (570 out of the 2,294). This was done to reduce the time it takes for the benchmark to finish, the same strategy the authors used in the original paper.
>
> Devin successfully resolved 79 of the 570 issues, giving a 13.86% success rate. This is significantly higher than even the previous best assisted system (Claude 2) of 4.80%.
>
> The baselines in this plot are evaluated in the “assisted” setting, where the model is provided with the exact file it needs to edit. Baselines perform worse in the “unassisted” setting, where a separate retrieval system selects the files for the LLM to edit (the best model is Claude 2 + BM25 retrieval with 1.96%).
>
> Because neither unassisted nor assisted is strictly comparable to the agent setting, where Devin is given the entire repo and can navigate the files freely, we choose the stronger numbers for the baseline comparison. We believe that running an agent end to end is the more natural setting for SWE-bench, as it’s more similar to real-world software development. We anticipate more agent results in this setting going forward.
>
> 私たちは Devin を、SWE-benchmark のテストセットの 25％（2,294 件中 570 件）をランダムに選んで評価しました。この方法は、ベンチマークの完了にかかる時間を短縮するために行われたもので、元の論文で著者たちが使用した戦略と同じです。
> Devin は 570 件のうち 79 件を解決し、13.86％の成功率を記録しました。これは、以前の最高の補助システム（Claude 2）の 4.80％を大きく上回る結果です。
>
> このグラフにおけるベースラインは、「補助（assisted）」設定で評価されています。この設定では、モデルには編集すべき正確なファイルが提供されます。ベースラインは、「非補助（unassisted）」設定では性能が低くなり、ここでは別の検索システムが LLM（大規模言語モデル）が編集すべきファイルを選択します（最良のモデルは Claude 2 + BM25 検索で 1.96％の成功率）。
>
> 「非補助」または「補助」設定は、Devin がリポジトリ全体を自由に操作できる「エージェント設定」とは厳密には比較できないため、ベースライン比較のために強い結果を選択しています。私たちは、エージェント設定での評価が SWE-bench にはより自然な設定だと考えています。なぜなら、現実のソフトウェア開発により近いためです。今後、この設定でのエージェントの結果が増えることを期待しています。

SWE-bench 公式の結果ではなくおそらく社内での計測結果ということがわかります。ここで、[SWE-bench 公式](https://www.swebench.com/index.html)を見てみるといろんな AI の比較が載っていますが、Cognition 社独自に用意したデータセットと大体同じ数のベンチマーク Verified(500 問)では Devin の 13.86%を超える AI モデルはいくつも出てきます。

### 補助と非補助

先ほどの引用文の中に補助と非補助という言葉が出てきました。この辺りも Cognition 社の意図を汲み取るのがなかなか難しいですが、

**「補助とは編集すべき正確なファイルが提供されているが、今回実施したベンチマークで Devin はリポジトリ情報を与えただけで issue を元に自ら問題となるファイルを探索し解決している」**

という風に読み取れます。なので Devin 以外のスコアは補助ありでこのスコアだけど Devin は非補助に近い形で実行していますよという表現だと思います。

おそらくファイルの検索や Git 操作等を行うプログラムを「エージェント」、実際のコード編集内容を考えるのが「AI モデル」という役割で分けているのが SWE-bench のやり方で、公式サイトの検索結果を見てみても「SWE-agent + GPT 4o」のスコアで 23.20%と出ているようなことからこのような認識でいいのかなと思います。

つまり SWE の公式に表示されているスコアは Devin のベンチマークを行った条件とほぼ同じではないのかと思います。現在の SWE-bench のスコアのトップが 62%で、OpenAI の o3[発表の資料](https://note.com/akikito/n/nc09faeaa4ba9)の中では 71.7%となっているので、それでいくと Devin のスコア自体は低すぎるのではないかなと思います。<br>
ただ、[こちらの記事](https://note.com/knaoe/n/n41014b43c055)によると、チェックマークがついてないものは信頼性がない場合があると言われていますがその場合の最大値でも 53.00%。とはいえ Devin のベンチマークの公開がされたのが 2024 年 3 月なので Devin も性能自体は上がっているのかもしれないです。Devin のサイトの下の方に

> Given the popularity of the open-source repos in the benchmark, Devin’s underlying models will contain data from these repositories. However, the baselines we are comparing to (Claude, GPT, Llama, etc.) face similar data contamination issues.
>
> In addition, effort needs to be put in to prevent agents from finding external information about these PRs and potentially copying the diff. During test setup, we remove the Github remote and all future commits from the repository so agents can’t access those directly. Agents with internet access could potentially find external information through other methods; we’ve manually inspected Devin’s successful runs to ensure this hasn’t happened.
>
> ベンチマークにおけるオープンソースリポジトリの人気を鑑みて、Devin の基盤となるモデルはこれらのリポジトリからのデータを含んでいます。しかし、比較対象としているベースライン（Claude、GPT、Llama など）も同様にデータ汚染の問題に直面しています。
>
> さらに、エージェントがこれらの PR（プルリクエスト）に関する外部情報を見つけて、差分をコピーしてしまう可能性を防ぐための努力が必要です。テストのセットアップ時には、GitHub のリモートとリポジトリからの今後のコミットを削除し、エージェントがそれらに直接アクセスできないようにしています。インターネットにアクセスできるエージェントは、他の方法で外部情報を見つける可能性もあります。そのため、Devin の成功した実行を手動で確認し、外部情報の利用がないことを確認しています。

と書いてあるため、「公式サイトのスコアって実は高めに出てるでしょ？うちは違いますよ。」という意図も汲み取れます。

## 評価基準

エージェントというものが最近出てきた概念なのでそもそも評価基準が曖昧なんだと思います。

ただ、エージェントを選ぶ側としては Devin のベンチマークについては少し疑問が残ります。やることは（きっと）やってくれますし、Azure で利用できるようになってますのでめちゃくちゃ怪しいってわけでもないと思いますが...

少し話はそれますが、 [ここ](https://www.publickey1.jp/blog/24/aidevin.html) に Devin と Azure の提携のポストに対する考察が書いてあります。

> マイクロソフト側が Cognition AI と提携するメリットは何でしょうか。
>
> いくつかの理由が考えられますが、筆者が考える最大の理由は、Devin を AWS や Google Cloud の手に渡さないようにしたのではないか、ということです。

なるほどと思いましたが、そのほかにも Devin が動いているサーバの IP アドレスを調べてみたところ Azure 上で動いていることがわかったので、双方にいろいろなメリットがあるのかなと思います。

## ソフトウェアエンジニアリングエージェントの未来

ソフトウェアエンジニアリングエージェントについては Devin が世界初というだけで、すでにオープンソースのエージェントも登場しています。先ほどスコアの例に挙げた SWE-agent もそれに当たります。発展途上の分野だとは思いますが、今後確実に発展していくのかなと思います。
