# 発表案まとめ

## 発表タイトル

Fine-Tuningっていつ使うの？
～小さなLLMをモデルルーターにして分かったこと～

## 発表時間

* 発表：10分
* 質疑：5分
* 対象：Fine-Tuningをまだ使ったことがない、または触り始めた初学者

# この発表で伝えたいこと

最も伝えたいメッセージは以下。

> Fine-Tuningするかどうかから考えるのではなく、
> 「モデルの何を変えたいのか？」から考える。

Fine-Tuningは単純に知識を追加するための手段ではない。

例えば、

* 外部の知識を参照させたい
* 決まった分類や出力形式を覚えさせたい
* 特定の回答傾向を好ませたい

では、それぞれ適した方法が異なる。

今回のモデルルーターでは、

> 「この入力なら、このカテゴリ・モデルを選ぶ」

という**判断・振る舞いをモデル側に学習させる**ことを試す。

# 問題設定

複数のLLMを利用するシステムを考える。

例えば、

* Coding向けモデル
* Database向けモデル
* Security向けモデル
* General向けモデル

などが存在する。

すると、新しい問題が生まれる。

> ユーザーから来た質問を、どのモデルに投げればいいのか？

例えば、

```text
PostgreSQLのクエリが遅い。
実行計画を確認しながら改善方法を教えてほしい。
```

という質問は、

* Coding
* Database
* General

のどこに振り分けるべきか。

この判断を行う小さなLLMを「モデルルーター」として作る。

# 解決方法

小型LLMに質問を入力し、決められたカテゴリを出力させる。

```text
User Question
     ↓
Small LLM Router
     ↓
┌──────────┐
│ Coding   │
│ Database │
│ Security │
│ General  │
└──────────┘
     ↓
Specialist LLM
```

まずPromptだけで分類させる。

その後、同じモデルに対してFine-Tuningを行う。

比較することで、

> Fine-Tuningによって何が変わるのか

を確認する。

# 実験

## 最低限行う比較

### 1. Promptのみ

小型Instructモデルに、

```text
以下の質問を4カテゴリから分類してください。

Coding
Database
Security
General
```

のようなPromptを与える。

Zero-shotまたはFew-shotで評価する。

### 2. LoRA Fine-Tuning

同じ分類問題について学習データを用意し、LoRAでFine-Tuningする。

```text
Question
↓
Category
```

という対応を学習させる。

### 3. 評価

同じTest Datasetに対して比較する。

見る指標は最低限、

* Accuracy
* Macro F1
* 誤分類例

とする。

数字だけでなく、

> どんな質問で間違えたのか

を見る。

# 今週作る実験用Repository

大規模なものにしない。

発表内容を検証するための、小さく再現可能なRepositoryにする。

## 想定構成

```text
ft-router-experiment/
├── README.md
├── data/
│   ├── train.jsonl
│   ├── dev.jsonl
│   └── test.jsonl
├── src/
│   ├── prompt_baseline.py
│   ├── train_lora.py
│   ├── evaluate.py
│   └── predict.py
├── results/
│   ├── prompt.json
│   ├── lora.json
│   └── report.md
└── pyproject.toml
```

# データセット

カテゴリは増やしすぎない。

3〜5カテゴリ程度にする。

例：

```text
Coding
Database
Security
General
```

データ件数も発表用なら大規模にする必要はない。

目安として、

```text
数百 ～ 1000件程度
```

から開始する。

重要なのは規模ではなく、

* Train / Dev / Testを分ける
* 同じデータをTestへ漏らさない
* カテゴリごとの件数を極端に偏らせない
* データソースそのものがラベルにならないよう注意する

こと。

# 注意するポイント

## Dataset Source Bias

例えば、

```text
Coding   → Magicoder
Security → Security専用Dataset
General  → Dolly
```

と完全にデータソースを分けると、

モデルが意味ではなく、

> 「この文章はMagicoderっぽいからCoding」

のようにデータセットの文体を学習する可能性がある。

可能であれば、

* 複数ソースを混ぜる
* 自作の小さなTest Setを作る
* Datasetとは別ソースの質問でも評価する

などを行う。

# 話の流れ

## 0:00〜1:00 問題

最初に具体的な質問を出す。

```text
PostgreSQLが遅いので、
原因調査と改善方法を教えてください。
```

そして聞く。

> Codingモデル？
> Databaseモデル？
> Generalモデル？

複数LLMを使うと、

> 「どのLLMを使うか」

という問題が出てくる。

## 1:00〜2:30 モデルルーター

そこで、小さなLLMに振り分けを担当させる。

```text
Question
  ↓
Router
  ↓
Specialist LLM
```

ここでモデルルーターという考え方を紹介する。

## 2:30〜4:00 まずPromptでやる

いきなりFine-Tuningしない。

まず、

> Promptだけで分類できるのでは？

を試す。

Zero-shot / Few-shotによる結果を紹介する。

## 4:00〜5:30 Fine-Tuningする

同じ小型LLMに、

```text
Question → Category
```

という分類を学習させる。

ここでLoRAを簡単に説明する。

詳細な数式や内部アルゴリズムには踏み込まない。

## 5:30〜7:00 結果

Prompt版とFine-Tuning版を比較する。

```text
                 Accuracy   Macro F1
Prompt               XX        XX
LoRA FT              XX        XX
```

さらに誤分類を数例紹介する。

重要なのは、

> FTしたら精度が上がった

だけで終わらせないこと。

## 7:00〜8:30 分かったこと

例えば、

* Promptだけでも十分なケースがある
* Fine-Tuningすると判断が安定する場合がある
* Datasetの品質が非常に重要
* ラベル境界が曖昧だとFTしても間違える
* FTそのものより評価設計が難しい

などを紹介する。

## 8:30〜9:30 具体から抽象へ

ここでモデルルーターから一般的な話に戻す。

```text
何を変えたい？

外部知識
   ↓
RAG

振る舞い・判断
   ↓
SFT / Fine-Tuning

回答の好み
   ↓
DPOなどのPreference Optimization
```

厳密な比較ではなく、役割のイメージとして紹介する。

## 9:30〜10:00 まとめ

最後は以下で締める。

> Fine-Tuningするかどうかを最初に決めるのではなく、
> 「モデルの何を変えたいのか？」から考える。

今回のモデルルーターでは、

> 「質問から適切なモデルを選ぶ」

という振る舞いを変えたかったため、Fine-Tuningを試した。

# やること

発表で扱う。

* モデルルーターとは何か
* なぜモデルルーターを作ったのか
* Promptによる分類
* LoRA Fine-Tuning
* PromptとFTの比較
* Accuracy / Macro F1
* 誤分類例
* Dataset作成で困ったこと
* Fine-Tuningして分かったこと
* RAG / SFT / DPOの大まかな役割
* Fine-Tuningを試すときの考え方
* 
# やらないこと

10分では扱わない。

## Fine-Tuningの網羅的解説

以下を詳しく説明しない。

* Full Fine-Tuning
* LoRA
* QLoRA
* DoRA
* Prefix Tuning
* Adapter Tuning

LoRAを今回使った方法として紹介するだけにする。

## DPOの詳細解説

DPOは、

> 「回答AとBならAを好む」

というPreference Optimizationの例として軽く紹介する程度。

数式やLossの説明はしない。

## RAGの詳細解説

RAGアーキテクチャやEmbedding、Vector DBなどには踏み込まない。

「知識を外部から与える方法」という位置付けだけ説明する。

## Full Fine-Tuningとの性能比較

実験していないなら比較結果を出さない。

質問された場合は、

> 今回はLoRAのみを対象としていて、Full Fine-Tuningとの比較はしていません。

と明確に答える。

## 「Fine-Tuningが最適だった」と断定する

今回の目的は、

> Fine-Tuningが最強だと証明する

ことではない。

あくまで、

> モデルルーターという問題にFine-Tuningを適用してみて、何が分かったか

を共有する。

# 想定される質疑

## なぜFine-Tuningしたの？

モデルに新しい知識を覚えさせたいのではなく、

> 「この質問ならどのモデルを選ぶか」

という判断を安定させたかったため。

## Promptだけではダメなの？

その疑問を検証するため、Prompt baselineと比較する。

結果次第では、

> Promptで十分だった

という結論でも問題ない。

## なぜLoRA？

小さい実験として、モデル全体を更新するより軽量にFine-Tuningできるため。

今回はFine-Tuning手法そのものの性能比較が目的ではない。

## RAGではダメ？

今回変えたいのは外部知識ではなく、

> 入力からどのモデルを選ぶかという判断

なので、RAGとは役割が異なる。

## どのくらいデータが必要？

今回の実験結果として使った件数を答える。

一般論として、

> 「何件あれば十分」

とは言い切らない。

タスクの難易度やカテゴリ境界、データ品質によって変わる。

## Full Fine-Tuningの方が良い？

今回は比較していない。

そのため、

> LoRAの方が優れている

とは言わない。

# TODO

## P0：発表成立に必須

* [ ] 実験用Repositoryを作成
* [ ] 分類カテゴリを決定
* [ ] Datasetを作成
* [ ] Train / Dev / Testを分割
* [ ] Prompt baselineを実装
* [ ] Zero-shotまたはFew-shot評価
* [ ] LoRA Fine-Tuningを実装
* [ ] LoRAモデルを学習
* [ ] 同一Test Datasetで評価
* [ ] Accuracyを算出
* [ ] Macro F1を算出
* [ ] 誤分類例を抽出
* [ ] Prompt vs FTの結果を表にまとめる

## P1：発表の品質を上げる

* [ ] Confusion Matrixを作る
* [ ] カテゴリ境界での誤分類を確認
* [ ] Dataset Source Biasを確認
* [ ] 学習データと異なるソースのTest質問を用意
* [ ] FTで改善した例を2〜3件抽出
* [ ] FTしても失敗した例を2〜3件抽出
* [ ] Promptの方が良かった例があれば残す
* [ ] 実験結果から「分かったこと」を3つ程度に整理

## P2：スライド作成

* [ ] 1枚目：タイトル
* [ ] 2枚目：どのモデルに投げる？
* [ ] 3枚目：モデルルーター
* [ ] 4枚目：まずPromptで試した
* [ ] 5枚目：Fine-Tuningしてみた
* [ ] 6枚目：比較結果
* [ ] 7枚目：誤分類・失敗例
* [ ] 8枚目：やって分かったこと
* [ ] 9枚目：RAG / SFT / DPOの位置付け
* [ ] 10枚目：まとめ

10分なので、実際には8〜10枚程度を上限にする。

## P3：質疑対策

* [ ] なぜLoRAか説明できるようにする
* [ ] LoRAとFine-Tuningの関係を整理する
* [ ] SFTとLoRAが同じ分類軸ではないことを整理する
* [ ] RAGとFTの違いを説明できるようにする
* [ ] DPOを一言で説明できるようにする
* [ ] Dataset件数を答えられるようにする
* [ ] 評価方法を説明できるようにする
* [ ] Full FTとの比較はしていないと明確にする
* [ ] 「FTが必要だったのか？」への回答を用意する

# 発表成功の基準

この発表では、

> Fine-Tuningについて詳しくなってもらう

ことを成功条件にしない。

聞いた人が最後に、

> 「とりあえずFTしてみよう」ではなく、
> 「自分が変えたいのは知識なのか、振る舞いなのか？」と考えられる

状態を目指す。

そのため、発表の中心はFine-Tuningの仕組みではなく、

**問題 → 仮説 → 実験 → 結果 → 失敗 → 一般化**

とする。
