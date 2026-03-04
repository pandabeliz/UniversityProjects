# ♟️ Chess T5 Seq2Seq — Transformer-Based Chess Player

A fine-tuned T5-base model that plays chess by framing move prediction as a sequence-to-sequence translation task: FEN board state → UCI move.
Built as part of the INFOMTALC (Transformers: Applications in Language and Communication) course at Utrecht University, Block 3, 2025–2026.

---

## Overview

This project implements a `TransformerPlayer` that inherits from a `Player` base class and implements `get_move(fen: str)` — a method that takes a FEN string representing the current board state and returns a valid UCI move string.

The player is evaluated in a tournament setting against:

- 🤖 Stockfish (Strong & Weak configurations)
- 🧠 Mistral-7B (LLM-based opponent)

The model is hosted on HuggingFace Hub: [`belpekkan/chess_T5_seq2seq`](https://huggingface.co/belpekkan/chess_T5_seq2seq)

---

## How It Works

Chess move prediction is treated as a translation problem:

```
Input (source):  "chess: rnbqkbnr/pppppppp/8/8/4P3/8/PPPP1PPP/RNBQKBNR b KQkq e3 0 1 legal: e7e5 d7d5 ..."
Output (target): "e7e5"
```

The encoder processes the FEN string (augmented with legal moves in pre-processing), and the decoder generates the predicted UCI move token by token.

---

## Model Architecture

| Component | Details |
|-----------|---------|
| Base Model | `google/t5-base` (encoder-decoder) |
| Task Framing | Seq2Seq translation (FEN → UCI) |
| Tokenizer | T5Tokenizer |
| Max Input Length | 256 tokens |
| Training Framework | HuggingFace `Seq2SeqTrainer` |

---

## Dataset

📦 All preprocessed training data is available on HuggingFace Datasets: [`belpekkan/chess_T5_training_data`](https://huggingface.co/datasets/belpekkan/chess_T5_training_data)

Training data is drawn from two sources:

### 1. 🌐 Lichess Standard Chess Games
- Source: HuggingFace `niklasf/lichess-big3-resolved` (streamed)
- Filtered to games with ELO ≥ 1800
- ~2,500+ games used across training runs

### 2. 👑 Hikaru Nakamura PGN Dataset
- Grandmaster-level games parsed from `Nakamura.pgn` — sourced from Kaggle
- Used as supplementary data to expose the model to high-quality strategic play
- Parsed using `python-chess` to extract (FEN-before-move, UCI move) pairs
- Further trained on endgame patterns to push the player toward wins rather than draws

### Combined Training
Both datasets are combined into `enhanced_training_data_v2.csv` to avoid catastrophic forgetting when fine-tuning on the Nakamura data alone.

---

## Input Format

Each training example is formatted as:

```
chess: <FEN_STRING> legal: <move1> <move2> <move3> ...
```

Legal moves are precomputed using `python-chess` and appended to the FEN prompt. This approach:

- Guides the model toward valid move selection
- Reduces illegal move fallbacks to zero during tournament play (which is penalised)
- Is fully within assignment rules (uses the assignment's own `python-chess` infrastructure)

---

## Training Setup

| Parameter | Value |
|-----------|-------|
| Platform | Google Colab (GPU) |
| Epochs | 10–15 (sets of 3 per session) |
| Batch Size | 8 (train), 8 (eval) |
| Max Input Length | 256 |
| Max Target Length | 10 |
| Model Hub | `belpekkan/chess_T5_seq2seq` |
| Persistence | `push_to_hub` after each run |

Models are saved to HuggingFace Hub after each training session so work persists across Colab sessions.

---

## Tournament Results

Results after continued training with combined Lichess + Nakamura dataset and override system (v4):

| Opponent | Result | T5 Fallbacks |
|----------|--------|--------------|
| Stockfish (Strong) | ½–½ Draw | 0 |
| Stockfish (Weak) | ½–½ Draw | 0 |
| Mistral-7B | **1–0 Win** ✅ | 0 |
| Random-1 | **1–0 Win** ✅ | 0 |
| Random-2 | **1–0 Win** ✅ | 0 |

✅ **0 illegal move fallbacks** across all tournament games  
✅ **3 wins, 2 draws** in latest tournament run

---

## TransformerPlayer — Revision History

The `TransformerPlayer` class evolved across four versions. Each version built directly on the previous one.

---

### v1 — Baseline
**Model:** `google/t5-small`

The initial proof-of-concept. Chess move prediction framed as seq2seq translation with a minimal input format:

```
chess: <FEN_STRING>
```

The model would generate a UCI move directly from the raw FEN. No legal move filtering, no overrides. High fallback rate due to frequent illegal move predictions.

---

### v2 — Legal Moves Augmentation + Larger Model
**Model:** `google/t5-base`

Two key improvements:

- **Upgraded to T5-base** for greater model capacity
- **Legal moves appended to input prompt**: `"chess: <FEN> legal: e2e4 d2d4 ..."` — precomputed via `python-chess`

The legal moves context guides the decoder to select from a valid candidate set rather than generate arbitrary tokens. Illegal move fallbacks dropped dramatically.

---

### v3 — Override System + Submission Fix
**Model:** `google/t5-base` (continued fine-tuning)

Introduced a layered deterministic override system on top of the model, plus a critical submission fix:

- **Submission fix:** `model_path` parameter given a default value pointing to the HuggingFace Hub repo — ensures `TransformerPlayer("Student")` works with a name-only argument as required by the tournament harness
- **Override 1 (Checkmate, 1-ply):** Before querying the model, scan all legal moves for an immediate checkmate. Play it instantly if found
- **Override 2 (Promotion):** Always promote pawns to queen; prefer promotions that deliver check
- **Override 3 (Winning position filter):** When material advantage exceeds 400cp, restrict moves offered to the model to captures and checks only — prevents aimless repositioning in won positions
- **Repetition penalty:** Persistent position history across moves; filters out moves that revisit previously seen positions

---

### v4 — 2-Ply Checkmate Detection + Stalemate Guard *(current)*
**Model:** `google/t5-base` (continued fine-tuning)

Upgraded Override 1 from 1-ply to 2-ply lookahead after tournament analysis revealed the model could not convert clearly won endgames (Q vs K, Q+piece vs K) — positions that rarely appear in training data because humans resign before reaching them.

- **Override 1 upgraded to 2-ply:** After failing to find an immediate mate-in-1, the override now checks whether any candidate move forces checkmate in 2 (i.e. every opponent reply leads to a position where we have a mate-in-1). Cost is O(our_moves × opponent_moves) board pushes — negligible in endgames where the opponent king has ≤ 8 moves
- **Stalemate guard:** Explicitly skips candidate moves that would leave the opponent with zero legal moves but no check (stalemate), preventing the override from accidentally throwing away a won position
- **Result:** Converted draws against Random opponents into wins; Stockfish-Weak still draws due to requiring deeper (3–5 ply) mating sequences that the model itself is not trained to execute

---

## Key Design Decisions

### ✅ Legal Moves as Input Context
Prepending legal moves to the FEN prompt dramatically reduces invalid outputs. The model learns to "choose" from a valid set rather than generate arbitrary tokens.

### ✅ FEN-Before-Move for Training Pairs
When parsing PGN files, it is essential to capture the board state *before* each move is played. Post-move FENs (as found in some CSV exports) would create incorrect input→output mappings.

### ✅ ELO Floor at 1800
A minimum ELO of 2000 caused severe Lichess streaming bottlenecks. Lowering to 1800 resolved this without meaningfully degrading training signal.

### ✅ Combined Dataset Training
Fine-tuning solely on Nakamura games risks catastrophic forgetting of patterns learned from Lichess. Combining both datasets preserves breadth while adding GM-level depth.

### ✅ Deterministic Overrides for Known Model Weaknesses
The T5 model is never trained on trivial endgames (Q vs K etc.) because humans resign before reaching them on Lichess. The override system compensates for this training data gap with deterministic logic, rather than attempting to train the model on synthetic endgame data alone.

---

## Project Structure

```
chess_T5_seq2seq/
│
├── train.ipynb          # Main training notebook (Google Colab)
├── player.py            # TransformerPlayer class definition (v4)
├── data_utils.py        # FEN parsing, legal move augmentation
│
└── README.md
```

---

## Acknowledgements

- **Course:** INFOMTALC — Transformers: Applications in Language and Communication, Utrecht University (Block 3, 2025–2026)
- **Libraries:** HuggingFace Transformers, `python-chess`, Lichess open dataset
- **Model hosting:** HuggingFace Hub — [`belpekkan/chess_T5_seq2seq`](https://huggingface.co/belpekkan/chess_T5_seq2seq)
- **Training data:** HuggingFace Datasets — [`belpekkan/chess_T5_training_data`](https://huggingface.co/datasets/belpekkan/chess_T5_training_data)
