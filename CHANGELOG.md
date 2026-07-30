# 最終流程稽核與修正紀錄

## 報告分工
- Word改為結論、文字解釋與窄版核心表格，不再放大型圖或完整矩陣。
- HTML納入完整圖形、可展開矩陣、各方案計分表、section合併／拆分對照與內容審查表。
- Excel保留逐題描述、所有候選模型、完整負荷矩陣、信度診斷、刪題迭代、未採用分支與正式計分表。
- `paper_ready_summary.txt`同時提供採統計建議、保留原section、概念上不能合併、保留重要題目及未刪題敏感度等論文寫法，並保留完整參考文獻。

## 因素數與小樣本決策
- 原定section數為優先工作方案，除非原定解出現多項強烈結構反證，且替代因素數獲平行分析及實質模型改善支持。
- 單因素不能因「不可能交叉負荷」而在評分上獲得不公平優勢。
- N低於設定門檻時，不以單次EFA永久推翻原定section，也不自動永久刪題；替代因素解標示為探索性敏感度結果。
- 原定方案、統計替代方案及未刪題原定方案均可輸出個別計分表與lavaan腳本。

## 題目與內容效度保護
- codebook新增`protect`與`content_note`；內容必要題可被保留並在報告中揭露統計問題。
- 補入修正後題目總分相關、alpha-if-deleted及ordinal-alpha-if-deleted。
- 新增section×實際因素交叉表、合併／拆分候選及題目文字審查表，避免僅靠數字命名合併後構念。
- 新增反向題方法因素警示與正式反向計分公式。

## 技術修正
- 修正自訂負荷門檻未完整傳入候選模型評分的問題。
- 修正替代因素方案在刪題迭代中可能被拉回原定因素數的問題。
- 修正AVE／CR負荷索引與缺失資料下alpha計算流程。
- 取消polychoric平行分析模擬次數的隱藏上限；正式預設提高為100次。
- lavaan腳本使用序位題`ordered=`與WLSMV；ESEM使用目前的EFA block語法。

## v0.2.0 — 2026-07-30

### Project repositioning
- Renamed the project from an EFA-only pipeline to **Questionnaire Quality & Measurement Structure Pipeline**.
- Renamed the executable to `questionnaire_quality_pipeline.py` and the default output directory to `questionnaire_quality_output`.
- Renamed report files to `Questionnaire_quality_report.docx`, `Questionnaire_quality_report.html`, and `Questionnaire_quality_details.xlsx`.

### Report separation
- Word is now a concise narrative-and-table report without embedded figures.
- HTML is the complete visual report and embeds all generated figures.
- Excel retains complete matrices, model comparisons, planned and alternative solutions, crosswalks, and diagnostics.
- `paper_ready_summary.txt` contains multiple manuscript scenarios and references.

### Theory-aware decisions
- Planned dimensions are prioritized unless strong counterevidence is present.
- Unsupported one-factor solutions cannot win because cross-loading penalties are structurally impossible.
- Planned and statistical alternative solutions are both preserved.
- Added section–factor crosswalks and merge-candidate tables.

### Content protection and small samples
- Added `protect` and `content_note` codebook fields.
- Automatic item deletion is disabled below the configurable small-sample threshold.
- The factor count remains fixed during an item-refinement run.

### Documentation
- Added an extensive bilingual README, methodological references, GitHub packaging files, citation metadata, license, contribution guidance, and a synthetic example dataset.
