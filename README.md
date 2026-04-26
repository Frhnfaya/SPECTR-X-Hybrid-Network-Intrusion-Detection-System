# SPECTR-X — Hybrid Network Intrusion Detection System

> **Spectrum of threats. X-factor for the unknown.**
> A research-grade NIDS combining Attention-based deep learning with XGBoost gradient boosting — built to see across the full spectrum of network attacks, including the ones nobody has labeled yet.



---

## 🌌 Origin Story — Why "SPECTR-X"?

Most intrusion detection systems are good at one thing — either deep semantic understanding (neural networks) **or** lightning-fast tabular reasoning (gradient boosting). I wanted both.

**SPECTR** = the full *spectrum* of network threats — from textbook port scans to subtle, slow, multi-stage intrusions.
**X** = the unknown. The zero-day. The variant nobody has labeled yet.

The name is a promise: detect the known *and* hold the line against the unknown. The crimson-on-black aesthetic of the project reflects that — clinical, focused, slightly threatening to anyone on the wrong side of the wire.

---

## 🎯 The Problem It Solves

Modern enterprise networks generate millions of packets per minute. A defender needs an IDS that:

- **Catches more than signatures** — adversaries mutate payloads faster than rules can be written.
- **Doesn't drown in false positives** — a 0.5% FPR at scale is still thousands of false alerts a day.
- **Explains itself** — analysts need to know *why* something was flagged, not just that it was.
- **Runs in production reality** — not just on a Kaggle notebook.

SPECTR-X was built around those four constraints from day one.

---

## 🏗️ Architecture

### The Hybrid Idea

```
            ┌─────────────────────┐
            │   Network Packets   │
            └──────────┬──────────┘
                       │
            ┌──────────▼──────────┐
            │  Feature Extraction │
            │   (flow + payload)  │
            └──────────┬──────────┘
                       │
        ┌──────────────┴──────────────┐
        │                             │
        ▼                             ▼
┌───────────────┐            ┌────────────────┐
│   Attention   │            │    XGBoost     │
│   Network     │            │   Classifier   │
│  (semantic    │            │  (tabular,     │
│   patterns)   │            │   high-speed)  │
└───────┬───────┘            └────────┬───────┘
        │                             │
        └──────────────┬──────────────┘
                       ▼
            ┌─────────────────────┐
            │   Hybrid Decision   │
            │   34-class output   │
            └─────────────────────┘
```

**Why hybrid?** Attention captures sequence and context (the *story* a packet tells). XGBoost captures sharp tabular splits (the *fingerprints* of known attack families). Combined, they cover each other's blind spots.

### Powers the Final Gate of InSpectra5

SPECTR-X v4.3 is the brain at the final gates of **InSpectra5** — a five-gate multi-packet inspection architecture I designed for layered defense. Earlier gates handle filtering, normalization, and triage; SPECTR-X handles the verdict.

---

## 📊 Performance

| Metric | v4.1 | v4.2 | v4.2.1 | **v4.3 (current)** |
|--------|------|------|--------|--------------------|
| Accuracy | 98.02% | 98.4% | 98.7% | **98.96%** |
| Attack Classes | 23 | 28 | 31 | **34** |
| Architecture | Attention only | Hybrid v1 | Hybrid v2 | Hybrid + tuned |
| Inference (single packet) | ~baseline | improved | improved | **production-ready** |

> All metrics are reproducible from the training pipeline (notebooks not public, but architecture and methodology are documented in `/docs`).

📸 **See `/docs/screenshots/` for confusion matrices, training curves, and per-class precision/recall.**

---

## 📈 Evolution Story

### v4.1 — The Foundation (98.02% / 23 classes)
A pure attention-based classifier. Worked well, but two problems showed up: it overfit on rare classes, and inference was heavier than I wanted for production.

### v4.2 — The Hybrid Hypothesis
*What if attention handled the contextual stuff and XGBoost handled the tabular fingerprints?* First hybrid prototype. Accuracy jumped, false positives dropped on rare classes.

### v4.2.1 — BB Edition
A leaner variant tuned for use inside **BountyStrike**. Same core, optimized for the bug-bounty scanning context where speed mattered more than coverage breadth.

### v4.3 — Full (current)
Pushed the hybrid further. Expanded to 34 attack classes, retrained with a richer feature set, and reached **98.96%** with stable per-class precision. This is the version that powers InSpectra5's final gates.

---

## 🔬 Technical Stack

- **Language**: Python 3.10+
- **ML**: PyTorch (attention model), XGBoost, scikit-learn
- **Data**: NumPy, Pandas, custom flow-extraction pipeline
- **Evaluation**: stratified k-fold, per-class precision/recall/F1, confusion matrices
- **Storage**: model artifacts versioned per release
- **Deployment target**: cloud-ready (AWS), containerizable

---

## 🧠 What I Learned Building This

- **Hybrid > monolith** when classes are heterogeneous. Some attacks are best caught by sequence understanding; others by sharp tabular thresholds.
- **Class imbalance is the silent killer** of NIDS accuracy. Reported accuracy on imbalanced data is meaningless without per-class precision/recall.
- **Feature engineering still matters** in 2026 — even with attention. The features you don't extract are the attacks you don't catch.
- **Iteration discipline** — versioning every model, every metric, every architectural change is what made v4.3 possible.

---

## 🛡️ Scope & Ethics

SPECTR-X is a **defensive** model. It is trained on public, ethically-sourced datasets and labeled attack traces. It is not, and will never be, used to generate or facilitate live attacks.

---

## 🔒 Source Code

This is a commercial research project. **The training pipeline, model weights, and inference code are private.** This README, the architecture diagrams in `/docs`, and the evaluation screenshots in `/docs/screenshots` are public so the work can be evaluated technically without exposing the implementation.

For technical deep-dives, model demos, or licensing conversations: contact below.

---

## 📞 Contact

**Seydu Farhan Moulana** — Founder, SPECTR-X
📧 frhnh8635@gmail.com
🔗 [linkedin.com/in/s-k-farhan-](https://www.linkedin.com/in/s-k-farhan-)
📍 Dubai, UAE

---


