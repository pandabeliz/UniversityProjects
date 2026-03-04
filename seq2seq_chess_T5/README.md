# ♟️ Chess T5 Seq2Seq — Transformer-Based Chess Player

> A fine-tuned T5-base model that plays chess by framing move prediction as a **sequence-to-sequence translation task**: FEN board state → UCI move.

Built as part of the **INFOMTALC** (Transformers: Applications in Language and Communication) course at **Utrecht University**, Block 3, 2025–2026.

---

## Overview

This project implements a `TransformerPlayer` that inherits from a `Player` base class and implements `get_move(fen: str)` — a method that takes a FEN string representing the current board state and returns a valid UCI move string.

The player is evaluated in a **tournament setting** against:
- 🤖 **Stockfish** (Strong & Weak configurations)
- 🧠 **Mistral-7B** (LLM-based opponent)

The model is hosted on HuggingFace Hub: [`belpekkan/chess_T5_seq2seq`](https://huggingface.co/belpekkan/chess_T5_seq2seq)

---

## How It Works

Chess move prediction is treated as a **translation problem** in my approach:

```
Input (source):  "chess: rnbqkbnr/pppppppp/8/8/4P3/8/PPPP1PPP/RNBQKBNR b KQkq e3 0 1 legal: e7e5 d7d5 ..."
Output (target): "e7e5"
```

The encoder processes the FEN string (augmented with legal moves in the pre-processing), and the decoder generates the predicted UCI move token by token.

---

## Model Architecture

| Component              | Details                              |
|------------------------|--------------------------------------|
| **Base Model**         | `google/t5-base` (encoder-decoder)   |
| **Task Framing**       | Seq2Seq translation (FEN → UCI)      |
| **Tokenizer**          | T5Tokenizer                          |
| **Max Input Length**   | 256 tokens                           |
| **Training Framework** | HuggingFace `Seq2SeqTrainer`         |

---

## Dataset

📦 All preprocessed training data is available on HuggingFace Datasets: [`belpekkan/chess_T5_training_data`](https://huggingface.co/datasets/belpekkan/chess_T5_training_data)

Training data is drawn from two sources:

### 1. 🌐 Lichess Standard Chess Games
- Source: HuggingFace [`niklasf/lichess-big3-resolved`](https://huggingface.co/datasets/niklasf/lichess-big3-resolved) (streamed)
- Filtered to games with **ELO ≥ 1800**
- ~2,500+ games used across training runs

### 2. 👑 Hikaru Nakamura PGN Dataset
- Grandmaster-level games parsed from `Nakamura.pgn` — sourced from [Kaggle](https://www.kaggle.com/datasets/zq1200/hikaru-nakamura-complete-chess-games-19972021)
- Used as supplementary data to expose the model to high-quality strategic play
- Parsed using `python-chess` to extract **(FEN-before-move, UCI move)** pairs
- Further trained on endgame patterns to push the player toward wins rather than draws

### Combined Training
Both datasets are combined into `enhanced_training_data_v2.csv` to avoid **catastrophic forgetting** when fine-tuning on the Nakamura data alone.

---

## Input Format

Each training example is formatted as:

```
chess: <FEN_STRING> legal: <move1> <move2> <move3> ...
```

Legal moves are precomputed using `python-chess` and appended to the FEN prompt. This approach:
- Guides the model toward **valid move selection**
- Reduces illegal move fallbacks to **zero** during tournament play (which is penalised)
- Is fully within assignment rules (uses the assignment's own `python-chess` infrastructure)

---

## Training Setup

| Parameter             | Value                                         |
|----------------------|-----------------------------------------------|
| **Platform**          | Google Colab (GPU)                            |
| **Epochs**            | 10–15 (sets of 3 during each training period) |
| **Batch Size**        | 8 (train), 8 (eval)                           |
| **Max Input Length**  | 256                                           |
| **Max Target Length** | 10                                            |
| **Model Hub**         | `belpekkan/chess_T5_seq2seq`                  |
| **Persistence**       | `push_to_hub` after each run                  |

Models are saved to HuggingFace Hub after each training session so work persists across Colab sessions.

---

## Tournament Results

Results after **3 training epochs** (~2,500 games):

| Opponent            | Result   |
|--------------------|----------|
| Stockfish (Strong)  | ½–½ Draw |
| Stockfish (Weak)    | ½–½ Draw |
| Mistral-7B          | 0–1 Loss |

- ✅ **0 illegal move fallbacks** during tournament play
- 📉 Validation loss was still decreasing at epoch 3 — further training expected to improve results

---

## Project Structure

```
chess_T5_seq2seq/
│
├── train.ipynb                      # Main training notebook (Google Colab)
├── player.py                        # TransformerPlayer class definition
├── data_utils.py                    # FEN parsing, legal move augmentation
│
└── README.md
```

---

## Key Design Decisions

### ✅ Legal Moves as Input Context
Prepending legal moves to the FEN prompt dramatically reduces invalid outputs. The model learns to "choose" from a valid set rather than generate arbitrary tokens.

### ✅ FEN-Before-Move for Training Pairs
When parsing PGN files, it's essential to capture the board state **before** each move is played. Post-move FENs (as found in some CSV exports) would create incorrect input→output mappings.

### ✅ ELO Floor at 1800
A minimum ELO of 2000 caused severe Lichess streaming bottlenecks. Lowering to 1800 resolved this without meaningfully degrading training signal.

### ✅ Combined Dataset Training
Fine-tuning solely on Nakamura games risks **catastrophic forgetting** of patterns learned from Lichess. Combining both datasets preserves breadth while adding GM-level depth.

---

## Acknowledgements

- **Course**: INFOMTALC — Transformers: Applications in Language and Communication, Utrecht University (Block 3, 2025–2026)
- **Libraries**: HuggingFace Transformers, `python-chess`, Lichess open dataset
- **Model hosting**: [HuggingFace Hub — belpekkan/chess_T5_seq2seq](https://huggingface.co/belpekkan/chess_T5_seq2seq)
- **Training data**: [HuggingFace Datasets — belpekkan/chess_T5_training_data](https://huggingface.co/datasets/belpekkan/chess_T5_training_data)
