# RaR(Rephrase and Respond)

ユーザーが入力したプロンプトをLLMが言い換えて、LLM自身が理解しやすくする手法。

# 背景

## 人間とLLMの「思考フレームの違い」

人間と大規模言語モデル（LLM）には、思考のフレーム（枠組み）に顕著な違いが存在します。思考の違いは、人間指示に基づくLLMのパフォーマンスに大きな影響を与えることがあります。例えば人間が明確だと感じる質問でも、LLMは異なる解釈をして、誤った応答をすることがあります。

人間の言葉や表現は、しばしば主観的な経験や文脈に依存しており、人間同士であっても異なる解釈がされることがあります。LLMも人間と同様に、特定の思考フレームに基づいて入力された情報を解釈します。ただしLLMの「思考フレーム」の構造や特徴は人間とは異なるため、誤解や誤った応答につながると考えられています。

## 誤解の解消

この誤解を解消するためには、LLMが人間からの質問を自分自身のフレームに合わせて言い換え、追加の詳細情報を組み込むことが効果的だということがわかりました。LLM自身による言い換えプロセスを通じて、質問はLLMにとってより明確な形に変換され、元の質問に内在する曖昧さが解消されます。

# 「RaR」の論理とロジック

## RaRメソッドの概要

今回カリフォルニア大学の研究者たちは、LLMに対して、人間の質問を自分で言い換えて応答させるためのプロンプト指示「RaR（Rephrase and Respond）」という新しい方法を提案しています。RaRは、パフォーマンスを改善するためのシンプルで効果的なプロンプト方法です。LLMが人間からの指示を言い換えて拡張を行い、単一のプロンプトで応答を提供します。

## RaRの主なポイント

### 「質問の意図」の明確化

質問の意図をより明確にすることにより、より正確な応答を促します。質問の意味の明瞭さが向上すると、LLMによる解釈の精度が高まります。

### One-stepとTwo-stepのオプション

RaRには、「One-step RaR」と「Two-step RaR」という2つのバリエーションがあります。One-step RaRは、単一のプロンプトを使用して言い換えた質問と回答を同時に生成します。一方、Two-step RaRは最初に質問を言い換え、その後で元の質問と言い換えた質問を組み合わせて応答します

### 教師なしでトレーニング不要

RaRは教師なしでトレーニングが不要な手法であり、すべての質問に適用可能です。

# プロンプトの例

## One-step

### 英語版
```
"{question}"
Given the above question, rephrase and expand it to help you
do better answering. Maintain all information in the original question. 
```

### 日本語版
```
{プロンプト}:
質問したい内容を入力

上記の{プロンプト}を、より良い回答ができるように言い換えたり、広げたりしてください。書き換えた質問の内容は、元の質問の全ての情報を維持します。
```



## Two-step

### 英語版

```
"{question}"
Given the above question, rephrase and expand it to help you
do better answering. Maintain all information in the original question. 
Please provide only rewritten prompts.
```

```
(original) {question}
(rephrased) {rephrased_question}
Use your answer for the rephrased question to answer the original question.
```


### 日本語版

```
質問したい内容を入力

上記の{プロンプト}を、より良い回答ができるように言い換えたり、広げたりしてください。{書き換えたプロンプト}の内容は、元のプロンプトの全ての情報を維持します。
{書き換えたプロンプト}の文章を表示してください。
```

```
{元のプロンプト}:
最初に入力したプロンプト

{書き換えたプロンプト}:
上記のプロンプトで書き換えられたプロンプト


{書き換えたプロンプト}に対する答えを使って、{元のプロンプト}に答えてください。
```

# 検証

## 検証1 (英語)

### Original question
```
You are an expert school principal, skilled in effectively managing faculty and staff. Draft 10-15 questions for a potential first grade Head Teacher for my K-12, all-girls', independent school that emphasizes community, joyful discovery, and life-long learning. The candidate is coming in for a first-round panel interview for a 8th grade Math teaching role. They have 5 years of previous teaching experience as an assistant teacher at a co-ed, public school with experience in middle school math teaching.
```

### Rephreased question

```
You are a highly experienced and competent school principal, renowned for your expertise in managing faculty and staff effectively. Your task is to prepare a set of interview questions for a candidate who has applied for the position of Head Teacher for the first grade at your K-12 all-girls' independent school. This school prides itself on fostering a sense of community, encouraging joyful discovery, and promoting lifelong learning. The candidate, who is currently in the running for this important role, is scheduled for a first-round panel interview. They are aiming to secure an 8th-grade Math teaching position. This individual brings with them a wealth of experience, having spent five years as an assistant teacher in a co-educational, public middle school, where they specialized in teaching math. Please draft 10-15 insightful and relevant questions that will help in evaluating the suitability of this candidate for the role in your school, taking into consideration the unique ethos and educational approach of your institution.
```


## 検証2 (日本語)

### 元の質問
```
あなたは教職員を効果的に管理することに長けた、専門家の校長です。コミュニティ、喜びの発見、生涯学習を重視するK-12、女子校の独立校で、1年生の教頭候補に10-15問の質問を作成してください。候補者は8年生の数学の教師として一次面接を受ける予定です。共学の公立学校でアシスタント教師として5年間の指導経験があり、中学校での数学の指導経験もあります。
```

###　書き換えた質問

```
あなたは、コミュニティ意識、喜びの探求、および生涯学習を重視するK-12の女子独立校で勤務する、教職員の管理に長けた校長です。現在、1年生のクラスを担当する教頭候補を探しており、その候補者は8年生の数学の教師として一次面接を控えています。この候補者は、共学の公立学校で5年間アシスタント教師として勤務し、中学校で数学を教えた経験があります。この候補者の適性を評価するための10から15の質問を考案してください。質問は、彼らの教育哲学、教室内での対応方法、コミュニティへの貢献、生徒への影響、および数学という教科の教え方に関するものであるべきです。
```

# 参照文献情報
- タイトル：Rephrase and Respond: Let Large Language Models Ask Better Questions for Themselves
- 著者：Yihe Deng, Weitong Zhang, Zixiang Chen, Quanquan Gu
- 所属：University of California, Los Angeles
- URL：https://doi.org/10.48550/arXiv.2311.04205
- GitHub：https://github.com/uclaml/Rephrase-and-Respond