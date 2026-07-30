# Validation Notes

The repository was smoke-tested on 2026-07-30 with a synthetic 12-item, four-dimension, five-point Likert dataset.

Checks completed:

- Python syntax compilation passed.
- Direct script execution passed.
- Editable installation and the `questionnaire-quality` console command passed using `--no-build-isolation` in the test environment.
- Pearson and polychoric analysis completed.
- Planned and statistical alternative solutions were exported.
- Word contained no embedded images.
- HTML embedded seven PNG figures.
- Excel contained planned-solution, statistical-alternative, crosswalk, reliability, validity, and full-diagnostics sheets.
- `paper_ready_summary.txt` contained multiple reporting scenarios and references.

The synthetic example is for software verification only and is not evidence that the program's heuristic thresholds are universally optimal. Users should inspect the report, the item content, and the methodological assumptions for each real dataset.
