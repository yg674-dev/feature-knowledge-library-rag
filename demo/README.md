# Live demo

**▶ [yg674-dev.github.io/feature-knowledge-library-rag/demo/](https://yg674-dev.github.io/feature-knowledge-library-rag/demo/?v=en)**

`index.html` is the v0.3_2 prototype plus a guided walkthrough. One self-contained file — no build
step, no server, no backend. To run it locally: `open index.html`.

## What it shows

The **Guided workflow** button at the bottom right steps through both flows in 13 steps, driving
the real UI rather than describing it:

| Steps | Flow | What it demonstrates |
| --- | --- | --- |
| 1 | Setup | The Feature List upgraded into a Knowledge Library — 7,024 features, 6,683 indexed, 341 failed |
| 2–6 | Flow A · Discovery | Ask AI open to everyone, one clarifying question on an ambiguous ask, cards labelled by fit rather than recommended, a shown confidence number, and an honest empty state |
| 7 | Flow A → B | Creating from an empty result is permission-gated |
| 8 | Flow A · Discovery | Final confirmation happens on the feature detail page |
| 9–12 | Flow B · Contribution | An interview instead of a 19-field form, 19/19 unlocking a lock rather than a submit, six evaluation checks where the failing one carries the value, and passing the gate as what makes the entry real |
| 13 | Close | Six places the system chooses to say less |

`?v=en` is a cache-buster; the interface opens in English either way, and the **EN / 中文** toggle in
the top bar switches it.

The PRD these flows implement is the [repo README](../README.md).
