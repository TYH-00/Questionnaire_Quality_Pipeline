# Questionnaire Quality & Measurement Structure Pipeline

## 問卷品質、測量結構與信效度評估工具

這是一套針對 **Likert-type questionnaire（Likert式問卷）** 的 Python 分析流程。它不只執行 **exploratory factor analysis, EFA（探索性因素分析）**，而是整合資料品質檢查、序位相關、因素數判斷、原定 section 與統計替代結構比較、保守刪題、信度估計、初步效度證據、Bootstrap 穩定性及後續 CFA／ESEM 腳本。

本工具特別適合以下常見研究情境：

- 問卷由研究者自行建構，發放前已有預定的 section／dimension（構面）。
- 研究完成後才需要檢查問卷是否可用於論文中的 correlation、regression、group comparison 或其他後續分析。
- 樣本可能不大，常見為 20–30 人，100 人以上已屬較充足。
- 研究者希望優先保留原定理論構面，而不是讓自動演算法任意改變問卷。
- 需要同時保留「理論導向方案」與「統計替代方案」，以便在論文中透明報告。

---

## 1. 這個工具能回答什麼？

程式會協助回答：

1. 資料是否存在大量缺答、零變異、極端偏態、直線作答或稀疏反應類別？
2. Likert 題應以 **Pearson correlation（皮爾森相關）** 或 **polychoric correlation（多分相關／序位潛在相關）** 為主？
3. 資料是否適合因素分析，例如 **Kaiser–Meyer–Olkin, KMO（取樣適切性）** 與 **Bartlett’s test of sphericity（Bartlett 球形檢定）**？
4. 平行分析、MAP、理論構面及候選模型分別建議多少因素？
5. 原定 section 數是否可保留？若不建議保留，具體反證是什麼？
6. 若統計上建議較少因素，哪些原 section 可能被合併到同一因素？
7. 題目是否有低因素負荷、交叉負荷、低共同性、低 MSA 或高度冗餘？
8. 哪些題目只是「修訂候選」，哪些題目在樣本足夠時才可考慮刪除？
9. 各分量表的 alpha、omega、ordinal alpha、ordinal omega 如何？
10. AVE、CR、HTMT 提供了哪些初步訊息，又有哪些不能據此宣稱的效度？
11. 問卷分數目前能否合理用於 correlation 或 regression？
12. 樣本是否足以做 CFA 或 ESEM，或只能產生供未來樣本使用的 R 腳本？

---

## 2. 這個工具不能單獨證明什麼？

即使 EFA、factor loading、AVE、HTMT、alpha 與 omega 的數字全部「看起來很好」，也不代表問卷已完成所有 validity（效度）驗證。

### 2.1 Validity 不是一個單一係數

依測驗與測量的當代觀點，**validity（效度）是支持特定分數解釋與使用方式的累積證據**，不是問卷本身永久擁有的一張合格證書。

本工具主要提供：

- **evidence based on internal structure（基於內部結構的證據）**
- **structural validity evidence（結構效度證據）**
- **internal consistency reliability evidence（內部一致性信度證據）**
- 題目與分量表之間的初步 **convergent evidence（聚合相關證據）**
- 分量表之間的初步 **discriminant evidence（區辨相關證據）**

本工具無法僅從一份作答 Excel 自動建立：

- **content validity（內容效度）**：題目是否完整涵蓋構念內容。
- **response-process evidence（反應歷程證據）**：受試者是否依預期理解題目。
- **criterion-related validity（效標關聯效度）**：與外部效標、行為或既有量表的關係。
- **predictive validity（預測效度）**：能否預測未來結果。
- **known-groups validity（已知群體效度）**：是否能區分理論上應不同的群體。
- **test–retest reliability（重測信度）**：跨時間穩定性。
- **measurement invariance（測量恆等性）**：不同性別、年級、語言或群體是否以相同方式測量。
- **independent-sample confirmation（獨立樣本確認）**：新樣本中的 CFA／ESEM 重現性。

因此報告採用以下較準確的說法：

> 本分析提供信度證據與基於內部結構的初步效度證據，而非宣稱問卷已完成全面效度驗證。

### 2.2 EFA、AVE 與 HTMT 的定位

- **EFA** 探索題目共同變異所形成的潛在結構，是 structural validity 的重要證據，但結果高度依賴目前樣本。
- **average variance extracted, AVE（平均變異抽取量）** 與 **composite reliability, CR（組合信度）** 原本多在 CFA／SEM 的明確測量模型下解讀。本工具由 EFA 負荷計算時，應視為 exploratory diagnostic（探索性診斷），不能取代 CFA。
- **heterotrait–monotrait ratio, HTMT（異質特質－同質方法比率）** 可協助辨識構面過度重疊，但本工具中的 HTMT 仍屬目前樣本的探索性區辨訊息。尤其樣本很小時，不宜僅以固定門檻判定構念已具有區辨效度。
- **Cronbach’s alpha** 或 **McDonald’s omega** 衡量的是分數精確度／可靠程度，不是構念內容是否正確，也不證明單向度。

---

## 3. 做完後能不能用於 correlation 或 regression？

### 3.1 不是「能跑」與「不能跑」的二分問題

統計軟體即使面對低信度或錯誤結構的分量表，仍然會算出 correlation coefficient（相關係數）或 regression coefficient（迴歸係數）。真正的問題是：

> 這些係數是否仍可解釋為研究者所主張的構念關係？

### 3.2 建議判斷表

| 內部結構 | 信度 | 後續 correlation／regression 建議 |
|---|---|---|
| 結構清楚，信度合理 | 合理 | 可以進行；報告本樣本的測量證據與限制。若屬新問卷，仍宜稱為初步或探索性使用。 |
| 結構清楚，但信度偏低 | 偏低 | 可作探索性分析，但測量誤差可能使關聯衰減、標準誤增加及統計功效下降。應報告信度信賴區間、進行敏感度分析，或考慮 latent-variable model（潛在變項模型）。 |
| 信度高，但結構不清楚 | 高 | 不應因 alpha／omega 高就直接使用。題目可能一致地測量了與原定構念不同的東西，或多個構念被混在一起。 |
| 原 section 彼此高度重疊 | 可能都高 | 若將高度重疊的分量表同時作為 predictors（預測變項），可能出現 multicollinearity（多重共線性）與係數不穩定。應檢查 VIF、tolerance、condition index，並考慮合併、上位因素或只使用理論上必要的預測變項。 |
| 結構與信度均明顯不佳 | 低 | 不建議將分量表分數作為已建立構念進行確認性推論。最多可作探索性、描述性或敏感度分析，並優先修訂問卷。 |

### 3.3 低信度不等於多重共線性

這兩個問題不同：

- **low reliability（低信度）** 是單一分數內含較多 measurement error（測量誤差）。
- **multicollinearity（多重共線性）** 是多個 regression predictors 彼此高度相關，導致各自的獨立效果難以分離。

低信度通常不會直接「造成」多重共線性。較常見的後果是：

- observed correlation（觀察相關）可能被 attenuation（衰減）。
- predictor 含 classical measurement error 時，迴歸斜率常向零偏誤，稱為 **regression dilution／attenuation bias（迴歸稀釋／衰減偏誤）**。
- outcome 含獨立隨機測量誤差時，通常會增加 residual variance（殘差變異）、降低精確度與統計功效；實際偏誤仍取決於測量誤差機制。

多重共線性較可能出現在以下情況：

- 原定兩個 section 在 EFA 中聚集成同一因素。
- 因素相關極高。
- HTMT 偏高。
- 兩個分量表題目內容高度重疊。
- 兩個分量表總分在目前樣本中高度相關。

### 3.4 如果效度或信度「沒有過」還能不能做 regression？

沒有任何單一的 `.70`、`.50` 或 `.85` 門檻能自動裁決所有研究。較合理的原則是：

#### 情況 A：信度略低，但構念很重要且題數很少

可以保留並作探索性分析，但應：

- 報告 omega／alpha 與 confidence interval（信賴區間）。
- 說明題數少可能限制係數。
- 避免對小效果或不顯著結果作過度解讀。
- 考慮使用 SEM／latent regression 將 measurement error 納入模型。
- 在新樣本中增加或改寫題目。

#### 情況 B：信度高，但因素結構與理論不一致

不宜直接把分量表當作原定構念。高信度可能只是題目高度重複。應先處理：

- 分量表是否其實形成其他因素。
- 是否應改名。
- 是否需要合併或拆分。
- 是否存在 wording／reverse-item method factor（措辭／反向題方法因素）。

#### 情況 C：統計建議合併，但理論上不能合併

可以暫時保留原 section，但必須透明報告：

- 統計替代方案建議多少因素。
- 哪些 section 在替代模型中共同落入同一因素。
- 為何理論上仍需分開。
- 原 section 的區辨性在本樣本中未獲充分支持。
- 相關／迴歸中的 section-specific effects（分量表獨立效果）只能作探索性解讀。

#### 情況 D：結構與信度均嚴重不佳

不建議用該分量表作正式 hypothesis testing（假設檢定）或確認性 regression。可以：

- 僅呈現題目層級描述統計。
- 將結果標為 pilot／exploratory evidence（前導／探索性證據）。
- 比較原定計分、替代計分與不刪題計分的 sensitivity analyses。
- 改寫問卷後重新蒐集資料。

---

## 4. 理論優先，但不隱藏反證

### 4.1 Planned dimensions（原定構面）是主要工作方案

若 `codebook.expected_factor` 填入四個不同 section 名稱，程式會將 4 視為 planned factor count（原定因素數）。

程式預設：

- 優先保留原定 section 數。
- 不用「多數決」讓平行分析、MAP 或模型評分直接取代理論。
- 不讓單因素模型因為「不可能出現交叉負荷」而取得不公平優勢。
- 樣本小於設定值時，不以一次 EFA 永久推翻原定結構。

### 4.2 什麼情況才視為原定因素數有強烈反證？

原定方案可能被建議調整，通常需要同時出現多項警示，例如：

- 某因素少於三個有效題目。
- **Heywood case（不當解）**，如負獨特性或共同性不合理。
- 因素間相關接近無法區分。
- RMSR 明顯偏高。
- 大量題目主要負荷低。
- 大量嚴重交叉負荷。
- 大量低共同性。
- 替代因素數同時獲平行分析支持，而且模型結構有實質改善。

### 4.3 原定方案與統計替代方案都會保留

若原定為 4 factors，而統計替代為 2 或 3 factors，輸出會同時包括：

- planned solution（原定方案）的負荷矩陣、因素相關、信度、AVE、HTMT。
- statistical alternative solution（統計替代方案）的相同結果。
- planned section × empirical factor（原 section × 實際因素）對照。
- 哪些 section 可能聚集到同一替代因素。
- `paper_ready_summary.txt` 中兩種論文寫法。

研究者可以基於理論保留 4 個 section，但不能寫成「EFA 證實四因素」。較正確的說法是：

> The planned four-subscale scoring structure was retained on theoretical grounds, whereas the more parsimonious factor solution was reported as a sensitivity analysis. The distinctiveness of the four subscales remains to be confirmed in an independent sample.

---

## 5. 如何判斷哪些 section 被合併？

程式建立 **section–factor crosswalk（section－因素對照表）**：

| planned section | empirical factor | 題目集中比例 |
|---|---|---:|
| Section A | F1 | 100% |
| Section B | F1 | 80% |
| Section C | F2 | 100% |
| Section D | F2 | 90% |

這表示 A 與 B 在目前資料中可能形成共同因素，C 與 D 可能形成另一共同因素。

但「統計上共同聚集」不等於「概念上可合併」。正式合併前必須確認：

1. 是否能提出合理的上位構念名稱？
2. 兩組題目是否描述相同心理歷程、行為或判斷？
3. 合併是否會失去研究問題需要的理論區別？
4. 文獻是否將兩構念視為可區分？
5. 題目措辭是否使受試者無法區分，而不是構念本身相同？

若概念上不能合併，保留原 section 並將區辨性不足列為限制，通常比機械合併更合理。

---

## 6. Item deletion（刪題）與內容效度保護

### 6.1 自動刪題檢查項目

- low primary loading（低主要負荷）
- cross-loading（交叉負荷）
- small loading gap（主要與次要負荷差距小）
- low communality（低共同性）
- low MSA（低個別取樣適切性）
- redundant correlation（高度冗餘）
- unexpected factor assignment（非預期因素歸屬）
- factor with too few items（因素題數不足）

程式一次只處理一題；若兩題都可能刪除，會比較刪除後模型的問題程度。

### 6.2 小樣本預設不永久自動刪題

若 `N < disable_auto_deletion_below_n`，程式：

- 不永久自動刪題。
- 將問題題目列為 revision candidate（修訂候選）。
- 保留完整題目供研究者做內容審查。

這是因為 N = 20–30 的單次 EFA 非常容易受抽樣波動影響。

### 6.3 `protect` 與 `content_note`

在 `codebook` 中：

- `protect = TRUE`：內容必要題，不得由程式自動刪除。
- `content_note`：填寫保留理由，例如「唯一測量 AI 輸出監督行為的題目」。

保護題仍會顯示統計問題；`protect` 不是把問題藏起來，而是避免統計演算法刪掉不可替代的內容。

### 6.4 內容必要題統計較弱時

建議：

- 保留題目作暫時計分。
- 同時跑含題與不含題的 sensitivity analysis。
- 在論文中說明內容效度理由。
- 不把信度上升當成刪題的唯一依據。
- 優先進行認知訪談、措辭改寫、增加平行題及新樣本驗證。

---

## 7. Analysis workflow（分析流程）

### Stage 1. Input validation（輸入檢查）

- 工作表及欄名檢查。
- 題目欄與 codebook 配對。
- 反向題計分。
- Likert 範圍檢查。

### Stage 2. Data-quality checks（資料品質檢查）

- missingness（遺漏）
- zero variance（零變異）
- response-category frequency（反應類別頻率）
- skewness／kurtosis（偏態／峰度）
- floor／ceiling effects（地板／天花板效應）
- straightlining（直線作答）
- longstring（長串相同作答）

只有過量缺答會依設定自動排除；直線作答預設只標示，不自動刪除受試者。

### Stage 3. Correlation matrices（相關矩陣）

同時計算：

- Pearson correlation matrix
- polychoric correlation matrix

主方法依 Likert 類別數、偏態、稀疏類別、估計邊界與 positive-definite adjustment（正定修正）判斷；另一方法作 sensitivity analysis。

### Stage 4. Factorability（因素分析適切性）

- KMO
- item-level MSA
- Bartlett’s test
- correlation-matrix diagnostics

### Stage 5. Factor retention（因素保留數）

- Pearson parallel analysis
- ordinal／polychoric parallel analysis
- Velicer MAP
- revised MAP
- scree plot
- planned theoretical dimensions
- candidate-model diagnostics

### Stage 6. Extraction and rotation（萃取與轉軸）

- principal axis factoring, PAF（主軸因素法）
- oblimin oblique rotation（Oblimin 斜交轉軸）
- varimax orthogonal rotation（Varimax 正交轉軸）

社會科學構念通常可能相關，因此預設由斜交解開始；正交解僅在因素相關很低且結果較簡潔時採用。

### Stage 7. Item refinement（題目修整）

- 一次一題。
- 固定當前方案的 factor count，不在刪題過程中任意改變因素數。
- 尊重 `protect`。
- 保留替代刪題結果。

### Stage 8. Reliability（信度）

各因素分別估計：

- Cronbach’s alpha
- ordinal alpha
- McDonald’s omega
- ordinal omega
- bootstrap confidence intervals

### Stage 9. Preliminary validity evidence（初步效度證據）

- factor loadings
- communalities
- AVE（探索性）
- CR（探索性）
- HTMT（探索性）
- planned–empirical crosswalk
- merge candidates

### Stage 10. Stability and follow-up（穩定性與後續分析）

- bootstrap factor-assignment stability
- CFA feasibility statement
- ESEM feasibility statement
- lavaan R scripts

---

## 8. Input Excel format（輸入格式）

使用 `questionnaire_input_template.xlsx`。

### 8.1 `responses` sheet

- 一列一位受試者。
- 一欄一題。
- 缺答留白，不要填 0。
- ID、班級、性別、年級、Group 等背景變項可保留，但不要列入 codebook。
- `Group` 只是選填背景變項，不參與 EFA。

### 8.2 `codebook` sheet

| 欄位 | 中文說明 | English meaning |
|---|---|---|
| `item` | 題號，須與 responses 欄名完全一致 | item variable name |
| `item_text` | 完整題目文字 | full item wording |
| `expected_factor` | 原定 section／構面名稱 | planned dimension/subscale |
| `reverse` | 是否為反向題 | reverse-scored item |
| `likert_min` | 量尺下限 | minimum response value |
| `likert_max` | 量尺上限 | maximum response value |
| `include` | 是否納入分析 | include in analysis |
| `protect` | 是否為不可自動刪除的內容必要題 | content-essential protected item |
| `content_note` | 題目內容或保留理由 | content-validity note |

`expected_factor` 應填真正的預定構面名稱，例如「AI委派能力」「AI監督能力」「AI信任」，不必維持 `Factor_A`、`Factor_B`。

---

## 9. Installation（安裝）

### 方法 A：requirements.txt

```bash
python -m venv .venv
source .venv/bin/activate
# Windows: .venv\Scripts\activate

pip install -r requirements.txt
```

### 方法 B：以 GitHub 專案安裝

```bash
pip install -e .
```

安裝後可使用 console command：

```bash
questionnaire-quality your_questionnaire.xlsx \
  --config config_template.yaml \
  --output questionnaire_quality_output
```

也可以直接執行 Python 檔：

```bash
python questionnaire_quality_pipeline.py your_questionnaire.xlsx \
  --config config_template.yaml \
  --output questionnaire_quality_output
```

---

## 10. Outputs（輸出檔案）

### `Questionnaire_quality_report.docx`

精簡、適合快速閱讀與存檔：

- 結論摘要
- 問卷是否可用
- 原定與建議因素數
- 核心模型比較
- 刪題與保留題
- 最終題目配置
- 信度與初步效度摘要
- CFA／ESEM 建議

Word 不放大型圖，避免圖片裁切或版面跑掉。所有表格都有文字說明。

### `Questionnaire_quality_report.html`

完整圖文版：

- scree／parallel-analysis plot
- final loading heatmap
- planned-solution heatmap
- statistical-alternative heatmap
- factor-correlation heatmap
- reliability plot
- bootstrap-stability plot
- 完整候選模型表
- 原定與替代方案題目表
- section–factor crosswalk
- merge candidates

圖形以 base64 嵌入單一 HTML，可直接分享或保存。

### `Questionnaire_quality_details.xlsx`

完整數值附檔：

- 描述統計與反應頻率
- Pearson／polychoric matrices
- KMO／MSA
- parallel analysis／MAP
- 所有候選模型
- planned solution
- statistical alternative solution
- crosswalk／merge candidates
- item-refinement logs
- complete pattern／structure／residual matrices
- reliability／AVE／CR／HTMT
- bootstrap stability

### `paper_ready_summary.txt`

包括：

- 共同方法段落
- 採主要統計方案的寫法
- 基於理論保留原 section 的寫法
- 概念上不能合併 section 的寫法
- 保留內容必要弱題的寫法
- correlation／regression 使用聲明
- 參考文獻

這些段落是可改寫模板，不應不經檢查直接貼入論文。

### `CFA_lavaan.R` 與 `ESEM_lavaan.R`

- Likert 題以 `ordered=` 指定。
- CFA 使用 WLSMV。
- ESEM 使用 lavaan EFA block。
- 樣本不足時腳本仍可供未來新樣本使用，但目前報告不會把同樣本 CFA 稱為獨立驗證。

---

## 11. Configuration（設定）

核心設定見 `config_template.yaml`。

### 原定因素數保護

```yaml
prioritize_planned_dimensions: true
preserve_planned_below_n: 50
```

### 小樣本不自動刪題

```yaml
disable_auto_deletion_below_n: 50
```

### 原定因素數的強烈反證門檻

```yaml
planned_factor_correlation_critical: 0.90
planned_rmsr_critical: 0.10
planned_problem_fraction_critical: 0.30
```

這些是 decision rules（決策規則），不是普遍真理。研究者應依領域、題目數與研究目的調整。

### 因素負荷與交叉負荷

```yaml
thresholds:
  loading_minimum: 0.32
  loading_preferred: 0.40
  cross_loading: 0.30
  loading_gap: 0.20
```

不應只因題目低於單一門檻就刪除；程式會綜合多項證據。

---

## 12. Recommended reporting language（建議用語）

### 可以說

- 「結果提供基於內部結構的初步效度證據。」
- 「本樣本中的 EFA 支持／部分支持某因素結構。」
- 「分量表在本樣本中呈現可接受／有限的內部一致性。」
- 「統計替代方案顯示 Section A 與 Section B 的題目反應未被清楚區分。」
- 「原定計分基於理論保留，因素區分仍待獨立樣本驗證。」

### 不建議說

- 「EFA 證明問卷有效。」
- 「AVE > .50，所以構念效度已完成。」
- 「HTMT < .85，所以所有區辨效度問題都不存在。」
- 「alpha > .70，所以問卷一定是單因素。」
- 「程式刪掉的題目一定不重要。」
- 「同一樣本 CFA 驗證了 EFA 結果。」

---

## 13. Sample-size policy（樣本數政策）

固定的「每題 5 人」或「每題 10 人」不能涵蓋所有情況。所需樣本與以下條件有關：

- communality
- loading magnitude
- factors per item
- items per factor
- cross-loadings
- response-category sparsity
- factor correlations
- model complexity

本工具採保守政策：

- `N < 50`：不自動永久刪題；原定 section 作暫時工作方案；替代解作探索性敏感度分析。
- `50 ≤ N < 100`：可做 EFA，但仍強調 bootstrap 穩定性，不切分樣本。
- `100 ≤ N < 200`：EFA較可行；同樣本 CFA 只能作診斷。
- 較大且有獨立樣本：才適合把 CFA／ESEM 當確認性證據。

這些範圍是程式的保守操作政策，不是學科通用硬門檻。

---

## 14. Reproducibility and privacy（可重現性與隱私）

- `random_seed` 固定模擬與 bootstrap 結果。
- `analysis_manifest.json` 記錄主方法、保留題目、因素數、轉軸及輸出檔。
- 程式在本機讀取 Excel，不需要把問卷資料傳至外部服務。
- GitHub 範例資料應使用 synthetic data（合成資料），不要上傳含個資或可識別受試者的原始資料。

---

## 15. Repository structure（建議 GitHub 結構）

```text
questionnaire-quality-pipeline/
├── README.md
├── questionnaire_quality_pipeline.py
├── questionnaire_input_template.xlsx
├── config_template.yaml
├── requirements.txt
├── pyproject.toml
├── REFERENCES.md
├── CHANGELOG.md
├── CITATION.cff
├── LICENSE
├── .gitignore
└── example_synthetic_input.xlsx
```

---

## 16. Important methodological references（重要方法文獻）

完整書目見 `REFERENCES.md`。核心依據包括：

- AERA, APA, & NCME. (2014). *Standards for Educational and Psychological Testing*.
- Fabrigar et al. (1999). Evaluating the use of exploratory factor analysis.
- MacCallum et al. (1999). Sample size in factor analysis.
- Hayton et al. (2004). Parallel analysis.
- Garrido et al. (2013). Parallel analysis with ordinal variables.
- Worthington and Whittaker (2006). Scale development recommendations.
- McNeish (2018). Reliability and coefficient omega.
- Mokkink et al. (2010) and COSMIN guidance on measurement properties.
- Fornell and Larcker (1981). AVE and measurement models.
- Henseler, Ringle, and Sarstedt (2015). HTMT.
- Measurement-error literature on attenuation and regression dilution.

---

## 17. Final interpretation principle（最終判讀原則）

本工具不把問卷品質簡化成「通過／不通過」。最終判斷應同時考量：

1. theory（理論）
2. item content（題目內容）
3. response process（受試者理解方式）
4. internal structure（內部結構）
5. reliability（信度）
6. relations with external variables（與外部變項的關係）
7. sample stability and generalizability（樣本穩定性與可推廣性）
8. consequences of score use（分數使用後果）

程式的角色是讓這些決策更透明、可重現且可報告，而不是取代研究者的專業判斷。
