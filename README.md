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

### 2. EFA、AVE 與 HTMT 的定位

- **EFA** 探索題目共同變異所形成的潛在結構，是 structural validity 的重要證據，但結果高度依賴目前樣本。
- **average variance extracted, AVE（平均變異抽取量）** 與 **composite reliability, CR（組合信度）** 原本多在 CFA／SEM 的明確測量模型下解讀。本工具由 EFA 負荷計算時，應視為 exploratory diagnostic（探索性診斷），不能取代 CFA。
- **heterotrait–monotrait ratio, HTMT（異質特質－同質方法比率）** 可協助辨識構面過度重疊，但本工具中的 HTMT 仍屬目前樣本的探索性區辨訊息。尤其樣本很小時，不宜僅以固定門檻判定構念已具有區辨效度。
- **Cronbach’s alpha** 或 **McDonald’s omega** 衡量的是分數精確度／可靠程度，不是構念內容是否正確，也不證明單向度。

---

## 3. Planned dimensions（原定構面）是主要工作方案

若 `codebook.expected_factor` 填入四個不同 section 名稱，程式會將 4 視為 planned factor count（原定因素數）。

程式預設：

- 優先保留原定 section 數。
- 不用「多數決」讓平行分析、MAP 或模型評分直接取代理論。
- 不讓單因素模型因為「不可能出現交叉負荷」而取得不公平優勢。
- 樣本小於設定值時，不以一次 EFA 永久推翻原定結構。

---

## 4. 如何判斷哪些 section 被合併？

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

## 5. Item deletion（刪題）與內容效度保護

### 5.1 自動刪題檢查項目

- low primary loading（低主要負荷）
- cross-loading（交叉負荷）
- small loading gap（主要與次要負荷差距小）
- low communality（低共同性）
- low MSA（低個別取樣適切性）
- redundant correlation（高度冗餘）
- unexpected factor assignment（非預期因素歸屬）
- factor with too few items（因素題數不足）

程式一次只處理一題；若兩題都可能刪除，會比較刪除後模型的問題程度。

### 5.2 小樣本預設不永久自動刪題

若 `N < disable_auto_deletion_below_n`，程式：

- 不永久自動刪題。
- 將問題題目列為 revision candidate（修訂候選）。
- 保留完整題目供研究者做內容審查。

這是因為 N = 20–30 的單次 EFA 非常容易受抽樣波動影響。

### 5.3 `protect` 與 `content_note`

在 `codebook` 中：

- `protect = TRUE`：內容必要題，不得由程式自動刪除。
- `content_note`：填寫保留理由，例如「唯一測量 AI 輸出監督行為的題目」。

保護題仍會顯示統計問題；`protect` 不是把問題藏起來，而是避免統計演算法刪掉不可替代的內容。

### 5.4 內容必要題統計較弱時

建議：

- 保留題目作暫時計分。
- 同時跑含題與不含題的 sensitivity analysis。
- 在論文中說明內容效度理由。
- 不把信度上升當成刪題的唯一依據。
- 優先進行認知訪談、措辭改寫、增加平行題及新樣本驗證。

---

## 5. Analysis workflow（分析流程）

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

## 6. Input Excel format（輸入格式）

使用 `questionnaire_input_template.xlsx`。

### 6.1 `responses` sheet

- 一列一位受試者。
- 一欄一題。
- 缺答留白，不要填 0。
- ID、班級、性別、年級、Group 等背景變項可保留，但不要列入 codebook。
- `Group` 只是選填背景變項，不參與 EFA。

### 6.2 `codebook` sheet

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

## 7. Installation（安裝）

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

## 8. Outputs（輸出檔案）

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

## 9. Configuration（設定）

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

## 10. Sample-size policy（樣本數政策）

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

## 11. Reproducibility and privacy（可重現性與隱私）

- `random_seed` 固定模擬與 bootstrap 結果。
- `analysis_manifest.json` 記錄主方法、保留題目、因素數、轉軸及輸出檔。
- 程式在本機讀取 Excel，不需要把問卷資料傳至外部服務。
- GitHub 範例資料應使用 synthetic data（合成資料）。

---

## 12. Important methodological references（重要方法文獻）

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

## 13. Final interpretation principle（最終判讀原則）

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
