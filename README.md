# CognitiveTrack

A responsive longitudinal cognitive assessment prototype with synthetic demonstration data. Open `dist/index.html`, or run `node preview.cjs` and visit `http://127.0.0.1:8765`.

## Features

Patient directory with search and status filtering; chronological assessment entry; individual baseline comparison; interactive domain trajectories; reaction-time monitoring; explainable alerts with persisted review notes; patient history; HTML patient reports, CSV assessments and JSON backup/restore. All data is stored in browser localStorage on the current origin. Local-preview data and hosted-site data are separate.

## Interpretation

The earliest assessment of the same protocol as the latest assessment is the baseline. Manual scores and Games v1 scores are never mixed in a trajectory or change calculation. Composite is the rounded unweighted mean of four 0–100 domain scores. Relative score deterioration is `(baseline - latest) / baseline * 100`; reaction-time deterioration is `(latest - baseline) / baseline * 100`. Zero score baselines are excluded from relative comparisons. The default illustrative review threshold is 10%, and monitor status begins at half the configured threshold. Reviews do not alter the underlying score classification. New assessment data invalidates a patient's old review acknowledgments and recomputes their trajectory.

## Interactive activities

Open **Activities** or **Play assessments**, choose a demonstration participant, and complete the four original tasks. Each has instructions, unscored practice, keyboard and pointer/touch input, and an explainable result. Review all scores and enter testing conditions before saving to the patient's timeline.

| Domain | Task | Fixed Games v1 scoring |
| --- | --- | --- |
| Memory | Sequence garden: reproduce 3-, 4-, 5-, and 6-tile sequences | Correct positions / 18 × 100; 30-second recall limit per round |
| Attention | Target watch: respond to 12 circles, withhold on 8 crosses | 50 × (hit rate + correct-withhold rate); 1.5 seconds per display; reactions below 100 ms excluded from hits |
| Executive function | Rule switch: 16 odd/even or below/above-5 classifications, including 8 switches | Correct / 16 × 100; 8-second limit per trial; switch accuracy and median correct reaction times reported separately |
| Processing speed | Symbol match: use a visible symbol-number key for 30 seconds | max(0, correct − errors) / 30 × 100, capped at 100; the denominator is an arbitrary demonstration target |

The assessment reaction time is the median valid target-detection response. Saving requires all four tasks and at least 3 valid attention hits; missing reaction time is not fabricated. Switching away from the tab aborts an active task. A retake replaces the prior result, not a best-score selection. Raw responses, timing, input methods, protocol version, and attempt counts are retained in JSON backups. Only one Games v1 assessment per participant per date is accepted. Unsaved scores remain in memory in the current tab and trigger an unload warning; patient records persist in localStorage after saving. Use consistent conditions and devices for follow-up. Scores are neither percentiles nor diagnostic cutoffs.

This is a demonstrator, not a clinically validated diagnostic or reliable-change model. It does not include age-adjusted norms, psychometric measurement error, practice effects, authentication within the app, a patient database, cross-device synchronization or clinical-system integrations. Use synthetic data only. Real deployment for patient care requires a validated assessment protocol and analysis method, secure backend, access controls, audit trail, retention policy and appropriate clinical review. The sample timeline ends in November 2026.

## Verification

`node --check dist/app.js` and `node --check dist/games.js` check syntax. `node check.cjs` verifies baseline logic, domain and reaction changes, classification, review state, backup validation, zero baseline handling, escaping and date range filtering. `node check-games.cjs` verifies all four scoring formulas, false alarms, anticipations, missing response handling, score bounds, rule-switch counts, protocol isolation, and backup uniqueness. Browser QA completed all four tasks and saved a complete session; practice did not contribute to scored results.
