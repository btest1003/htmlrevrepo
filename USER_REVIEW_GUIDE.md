# chara-vocab End-to-End User Review Guide v0.5

Status: **DESIGN READY / UI IMPLEMENTATION NOT YET COMPLETE**

現在の `prototype-user-review-v1.html` は入力UIだけを確認する旧Prototypeであり、v0.5のEnd-to-End Review完成版ではありません。

## 目的

最重要の確認項目：

> 少ない入力で、自分が想像していたキャラクターと実際に会話できたか。

入力UIの使いやすさだけでは合否を決めない。

---

## Canonical Review Flow

```text
1. UIでキャラクターを入力
↓
2. UIが自己完結Portable Compiler Requestを生成
↓
3. Fresh AI Chat Aへ貼る
↓
4. Standalone Production RP Promptを取得
↓
5. Fresh AI Chat Bへ貼る
↓
6. 複数Scene / 複数Turnで実際に会話
↓
7. User Review
```

Chat A / Bは開発会話とは別のFresh Chatを推奨する。既存会話の背景知識が結果を補完するのを避けるため。

---

## UI Input Baseline

### Personality

```text
2〜4個推奨
6個上限目安
```

Stable Traitを基本とし、Advanced Candidateは「もう少し設定」等へ表示してHuman Reviewしてよい。

### Culture

v0.5 Baseline：

```text
1 触れる程度
2 好き
3 かなり好き
4 生活の一部
```

CultureはUIの中心入力。

`Core ★` は入力コスト削減候補として後で比較し、最初のEnd-to-End Baselineにはしない。

### Relationship / User Stance

別々に入力する。

### Adaptive

通常0問。

現時点では `感情豊か + 淡々` のみ1問候補。

### Free Note

選択肢で表現しにくい一点物に使用。Free Note使用自体を失敗扱いしない。

---

## 最初のReview Run

最初は自由に1キャラクターを作る。

見るもの：

```text
入力負荷
Portable Compiler Requestが正しく生成されたか
Production Promptが入力の意味を保持したか
実際のRP Responseが想像した人物に近いか
```

---

## Character Test Scene

最低限、異なる場面を5〜6個試す。

```text
A casual
B user vulnerability / emotion
C relationship-specific
D personality high-signal
E culture-relevant
F culture-irrelevant / neutral control
```

必要なら同じSceneを複数回生成し、model varianceも確認する。

固定Sceneだけでなく、その後2〜3Turn自由会話も行う。

---

## User Review Metrics

### Primary

```text
Character Fidelity 1〜5
想像した人物として会話できた感覚
```

### Input

```text
入力しやすさ
項目量
欲しいTrait / Cultureが見つかったか
迷った項目
Free Note依存
```

### Output

```text
Personality correctness
Culture naturalness
Relationship / User Stance correctness
Unwanted added personality
Missing selected feature
Naturalness
Consistency across turns
```

---

## Failure Classification

「キャラが違う」で終わらせず、可能なら次へ分類する。

```text
A. Input / UI
想像した人物を入力できなかった

B. Compiler Interpretation
入力した意味がProduction Promptへ正しく変換されなかった

C. Production Prompt
意味は理解されたが、実行指示として不足・過剰だった

D. RP Realization / Model Variance
Promptは妥当だが、RP AIのその出力だけ外れた
```

---

## Review中の注意

- Internal AI Regressionの高得点を理由にHuman Reviewの違和感を無視しない。
- Cultureが毎Turn見えることを成功条件にしない。無関係Sceneで消えるのが正しい場合がある。
- Traitも毎Turn演技させる必要はない。
- 明示していない人格が追加されたら記録する。
- 明示した極端な性質が勝手に善良化・弱化された場合も記録する。
- 1人Reviewの数値を統計的合否として扱わない。具体的な摩擦・Fidelity failure発見を優先する。

---

## Review後の修正優先順位

```text
1. Character Fidelity failure
2. Input UIで表現不能
3. Over-inference / missing feature
4. Culture naturalness
5. Relationship mismatch
6. Input Cost
7. UI polish
```

問題箇所を修正した後、必要なTargeted Regressionだけ実施する。

新しい内部Parameter研究をHuman Reviewより優先しない。

---

## v0.5 Review-ready条件

```text
[ ] UIからPersonality 2〜4推奨 / 6上限目安で入力可能
[ ] Culture 1〜4を入力可能
[ ] Relationship / User Stance / Free Note入力可能
[ ] 必要時だけAdaptive表示
[ ] Portable Compiler Requestを生成・コピー可能
[ ] Fresh Chat AでStandalone Production RP Prompt生成可能
[ ] Fresh Chat BでRP可能
[ ] Review結果を記録可能
```

現在の次工程は、既存UI PrototypeをこのFlowへ接続すること。
