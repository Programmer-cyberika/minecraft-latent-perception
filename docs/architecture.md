

### Project: **Action Recognition via Latent Representations + LSTM**

---

## ✅ High-Level Goal

Recognize *what action is happening on the Minecraft screen* using only screen frames — without direct environment access, logs, or game API.

The system will:

1. Compress raw game frames into **latent vectors** (vision encoder).
2. Feed sequences of latent vectors into an **LSTM**, to recognize the full action.
3. Output an action label (`"break_block"`, `"place_block"`, `"open_inventory"`, etc.).

This project is a component of a larger future agent (Stratus), but remains **standalone**, focused only on perception.

---

## 🧠 System Overview

```
+---------------------------+       +-------------------+        +--------------------+
|        Video Frames       | --->  |  Vision Encoder   | --->   |       LSTM         |
| (RGB capture of gameplay) |       | (Autoencoder/CNN) |        | (sequence memory)  |
+---------------------------+       +-------------------+        +---------+----------+
                                                                               |
                                                                               v
                                                                     +----------------+
                                                                     |  Action Label  |
                                                                     | (e.g. break)   |
                                                                     +----------------+
```

---

## 🧩 Components

### 1️⃣ **Vision Encoder (Latent Representation Generator)**

(May be Autoencoder / CNN / Vision Transformer — first version = autoencoder)

* Input: Single frame (H × W × 3 pixels)
* Output: Latent vector (compressed feature vector)
* Purpose: Extract meaningful visual representation without needing the full image.

Key idea:

> The model should understand “what the frame contains,” not every pixel.

Example:

```
Input frame → [0.12, -0.88, 0.33, ... , 0.55] (latent representation)
```

---

### 2️⃣ **LSTM (Action Sequence Interpreter)**

* Input: Sequence of latent vectors (frames over time)
* Output: Action class (one of the actions defined in `actions.json`)

Why LSTM?
Because actions are not defined by a single frame — they are defined by **how frames change over time**.

Example:

* Move forward → break block → particles appear → block disappears
  LSTM understands this *flow of time*.

Normal NN: only knows the current frame
LSTM: remembers previous frames as context

---

### 3️⃣ **Action Classifier**

* Classification layer (usually softmax)
* Takes final LSTM output → returns a labeled action

Example:

```
{
  "break_block": 0.92,
  "place_block": 0.03,
  "open_inventory": 0.01
}
```

---

## 🔧 Training Strategy

| Stage   | Model             | Train on                | Goal                              |
| ------- | ----------------- | ----------------------- | --------------------------------- |
| Stage 1 | Autoencoder       | Random Minecraft frames | Learn visual abstraction          |
| Stage 2 | LSTM + Classifier | Labeled video clips     | Learn temporal action recognition |


## 📌 Assumptions

* No need for Minecraft API access.
* Works purely from gameplay video / frame dumps.
* PPO / reinforcement learning comes later — not part of this repo.

---

## 🚧 Future (Not part of MVP)

These are *future layers* planned for the Stratus agent, not for this repo:

* Goal decider
* Task decomposition
* Full PPO-based decision-making

This repo = **only perception**.


### “Compress visual frames → remember sequence → infer action.”

