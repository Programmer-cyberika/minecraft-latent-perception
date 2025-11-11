# Minecraft Latent Perception 🎮🧠  
*(Component of the Stratus Agent – standalone research module)*

---

## 📌 Overview

This project explores **latent representation learning** + **temporal sequence modeling** for Minecraft gameplay frames.

Instead of feeding raw pixels into reinforcement learning models, this module:

1. Captures Minecraft screen frames (via ADB → Android Bedrock, or screen recording).
2. Passes them through a **vision encoder** (CNN / ViT / Autoencoder) → producing a compact latent vector.
3. Over a sequence of frames, an **LSTM (or transformer)** learns the temporal pattern.
4. A classifier / PPO policy predicts the **action happening in the frame**.

> In short: *pixels → latent vector → temporal model → action identification*.

---

## 🧠 Why?

Traditional RL learns everything directly from raw pixels.  
That means it has to *simultaneously* figure out:

- Spatial recognition ("what is happening?")
- Temporal consistency ("what just happened before?")
- Action → reward mapping

By splitting responsibilities:

| Module | Job |
|--------|-----|
| **Vision encoder** | Understand the scene (extract features) |
| **LSTM / Transformer** | Understand action sequence over time |
| **Policy / Classifier** | Output what action is happening |

This makes training:
- **faster**
- **more stable**
- **less compute hungry**

---

## 🔁 Why LSTM / Transformer?

Minecraft actions are not based on a single frame.

Example: breaking a block:
- Frame 1 → crosshair at wood
- Frame 2 → crack stage 1
- Frame 3 → crack stage 2
- Frame 4 → block drops

A single frame can't tell that the agent is *breaking a block*.  
But a sequence of latent vectors reveals the pattern.

### Simple diagram

```
Frame[t-3] Frame[t-2] Frame[t-1] Frame[t]
     │        │        │        │
     ▼        ▼        ▼        ▼
   Latent   Latent   Latent   Latent
    z1        z2        z3        z4
       └─────── into LSTM ───────┘
                   │
                   ▼
          Predicted Action
```

---

## 🚧 Current Scope (MVP)

- ✅ Capture frame → convert to latent vector
- ✅ Sequence model (LSTM/transformer ready)
- ⏳ Train classifier / PPO to map latent → action

Non-goals (in this repo):
- Crafting logic
- Inventory planning
- High-level reasoning

This repo is intentionally focused on **perception + action recognition**.

---

## 🏗 Architecture

```
Minecraft Frame (RGB)
        │
        ▼
  Vision Encoder (CNN/ViT/Autoencoder)
        │ latent vector z_t
        ▼
 LSTM / Transformer (sequence)
        │
        ▼
Action Classifier or PPO Policy
```

---

## 📚 Dataset Structure

```
dataset/
   ├── frames/
   │     ├── clip01/
   │     │      ├── frame_0001.png
   │     │      ├── frame_0002.png
   │     │      ├── ...
   ├── labels.csv   # clip_id → action mapping
```

Example `labels.csv`:

```
clip,action
clip01,break_block
clip02,move_forward
clip03,jump
```

---

## 🛠 Tech Stack

| Component | Choice |
|----------|--------|
| Frame capture | ADB-to-PC screen fetch |
| Latent encoder | Autoencoder / CNN / Vision Transformer |
| Sequence model | **LSTM / Transformer** |
| Policy (optional) | Stable Baselines PPO |
| Framework | PyTorch |

---

## 🧪 Roadmap

- [ ] Build dataset pipeline
- [ ] Train latent autoencoder
- [ ] Train LSTM over latent sequences
- [ ] Train action classifier or PPO
- [ ] Visualize latent space in 2D/3D (t-SNE / UMAP)

---

## 🤝 Contributions

This is a **public research experiment**.  
Feedback, issues, and PRs are welcome.

---

## 📄 License

MIT License — for open collaboration.

---

## ⭐ Acknowledgments

Inspired by:
- MineRL
- Vision-based RL research (DeepMind Atari, DreamerV3)
- Curiosity-driven exploration ideas

> “Understand the world first. Act second.”
