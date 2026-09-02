# chara-vocab End-to-End User Review Guide v0.5

Status: **E2E PROTOTYPE READY FOR MANUAL REVIEW**

対象ページ：`prototype-e2e-review-v0.5.html`

旧 `prototype-user-review-v1.html` は入力UIだけを確認する比較用Prototypeとして残す。

## 目的

最重要の確認項目：

> 少ない入力で、自分が想像していたキャラクターと実際に会話できたか。

入力UIの使いやすさだけでは合否を決めない。

---

## Canonical Review Flow

```text
1. prototype-e2e-review-v0.5.html でキャラクターを入力
↓
2. UIが自己完結Portable Compiler Requestを生成
↓
3. [Compiler用Promptをコピー]
↓
4. Fresh AI Chat Aへ貼る
↓
5. Standalone Production RP Promptを取得
↓
6. そのPromptをFresh AI Chat Bへ貼る
↓
7. 複数Scene / 複数Turnで実際に会話
↓
8. UIのReview欄へ結果を記録
↓
9. [Review保存] または [Review Packetをコピー]
```

Chat A / Bは開発会話とは別のFresh Chatを推奨する。既存会話の背景知識が結果を補完するのを避けるため。

---

## UI Input Baseline

### Personality

```text
2〜4個推奨
最大6個
```

Stable Traitを基本とし、Advanced Candidateは「もう少し設定」へ表示する。Candidateを選べることはStable昇格を意味しない。

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

現時点では `感情豊か + 淡々` のみ1問候補。未回答でもSoft DefaultでCompileできる。

### Free Note

選択肢で表現しにくい一点物に使用。Free Note使用自体を失敗扱いしない。

---

## Portable Compiler Request

ページは以下を自動で結合する。

```text
Compiler共通Rule
選択TraitのCanonical Contribution / Boundary
該当Mixed Rule
選択CultureのFeature / Level / Suppression
Relationship / User Stance
Free Note / Detail Rule
Deception Guard
Sparse Character Input
Consistency Checklist
Output Contract
```

UI自身は人格を解釈しない。選択されたCanonical Snippetを機械的に結合するだけ。

---

## 最初のReview Run

最初の代表ケース：

```text
年代: 10代
性別: 女性

Trait:
- 仕返し好き
- 意地悪
- 自分優先
- 落ち着いている
- 人見知り

Culture:
- 少女漫画
- X・短文SNS

Relationship:
- 知り合い

User Stance:
- 苦手
```

Culture Levelは最初のRunでは、少女漫画=2、X・短文SNS=4を暫定Baselineとして使える。Level自体もHuman Review対象なので、想像と合わなければ次Runで変更する。

---

## Character Test Scene

最低限、異なる場面を5〜6個試す。

```text
A casual
「今日なんか暇だな」

B user vulnerability / emotion
「今日ちょっと仕事で失敗した」

C relationship-specific
「なんか俺のこと苦手？」

D personality high-signal
「前にお前にやられたこと、まだ覚えてる？」

E culture-relevant
「少女漫画って何が面白いの？」

F culture-irrelevant control
「ところで富士山って何メートルだっけ？」
```

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
Traitの分かりやすさ
Cultureの分かりやすさ
```

### Output

```text
Personality correctness
Culture naturalness
Relationship / User Stance correctness
Naturalness
Consistency across turns
```

加えて自由記述で必ず確認する：

```text
Unwanted added personality
Missing selected feature
想像と違ったResponse
迷った入力
足りなかった項目
不要に感じた項目
```

Production RP Prompt、想像に近かったResponse、問題Responseは診断用として任意保存できる。

Compiler AI / RP AIのmodel名も記録する。

---

## Failure Classification

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
[x] Personality 2〜4推奨 / 最大6
[x] Culture 13種 × Level 1〜4
[x] Relationship / User Stance / Free Note
[x] 感情豊か + 淡々 のAdaptive候補
[x] Sparse Input Preview
[x] Portable Compiler Request生成
[x] ワンクリックCopy
[x] Compiler / RP model記録欄
[x] Production Prompt / Response診断保存欄
[x] Review localStorage保存
[x] Review Packetコピー
[ ] Fresh Chat Aで実際にProduction RP Promptを生成するHuman Run
[ ] Fresh Chat Bで実RPするHuman Run
```

次工程は、実際のFresh Chat A / Bを使った最初のEnd-to-End Human Review。