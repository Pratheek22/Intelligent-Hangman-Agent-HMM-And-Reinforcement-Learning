# 🧠 Hybrid HMM + Reinforcement Learning Agent for Hangman

### UE23CS352A: Machine Learning Hackathon

**Team Members**
- Pranav Rajesh Narayan — PES1UG23CS435  
- Priyman Jain — PES1UG23CS453  
- Rajat Ramakrishna Bhat — PES1UG23CS465  
- Pratheek J Gowda — PES1UG23CS448  

---

## 📘 Overview

This project implements a **hybrid intelligent system** that plays a Hangman-style word game using a combination of:
- **Hidden Markov Model (HMM)** for probabilistic language modeling, and  
- **Reinforcement Learning (Q-learning)** for adaptive letter-guessing strategies.  

The system trains on a large text corpus to estimate the probabilities of letter sequences and uses these probabilities to guide an RL agent through the game environment.

---

## ⚙️ System Design

### 1. **Language Model – Bigram HMM**
- Built from scratch using a custom `BigramHMM` class.  
- Estimates **transition** and **emission** probabilities between letters.  
- Implements the **forward–backward algorithm** in **log-space** to prevent numerical underflow.  
- Produces posterior probabilities for each possible letter in a word pattern, which are passed to the RL agent.

### 2. **Reinforcement Learning Agent**
- The environment (`HangmanEnv`) provides the agent with:
  - Current word pattern  
  - Previously guessed letters  
  - Remaining lives  
  - HMM posterior probability vector  
- The agent uses **tabular Q-learning** with an **epsilon-greedy** exploration policy.  
- Rewards:
  - ✅ +2 for each correct letter  
  - 🏆 +50 for completing a word  
  - ❌ -5 for wrong guesses  
  - ⚠️ -2 for repeated letters
- Evaluation conducted over 2000 simulated Hangman games to measure agent accuracy and convergence.


This reward design encourages exploration early on and accuracy later in training.

---

## 🔍 Exploration vs Exploitation

The QAgent controls exploration using a **decaying epsilon**:
- Starts at **0.6** for high exploration  
- Gradually decays to **0.05**  
- Balances between random guessing and leveraging learned Q-values  
- Combines Q-values and HMM posterior probabilities during exploitation for smarter decision-making.

---

## 📊 Results

| Metric | Earlier Model (Full HMM) | Final Model (BigramHMM) |
|:--|:--:|:--:|
| **HMM Design** | Full HMM (Forward–Backward–Viterbi); no smoothing | Bigram HMM with log-space computation & Laplace smoothing |
| **RL Agent** | Fixed reward, hybrid HMM-Q | Epsilon decay, structured rewards, smoother convergence |
| **Dataset Handling** | Raw corpus, noisy transitions | Cleaned corpus, Laplace smoothing |
| **Exploration Strategy** | Static epsilon | Decaying epsilon (0.6 → 0.05) |
| **Win Rate (2000 games)** | 18.95% (379 wins) | **33.75% (675 wins)** |

---

## 🚀 Future Improvements

- Extend to **trigram** or **position-specific** HMMs for deeper linguistic context.  
- Replace tabular Q-learning with:
  - **Double Q-learning**, or  
  - **Neural network–based Q-function approximation**.  
- Introduce **curriculum learning** — training on short words first, then progressively longer ones.  

---

## 🧩 Key Learnings

- Integrating probabilistic models (HMM) with reinforcement learning enables structured decision-making even under uncertainty.  
- Proper reward shaping and exploration decay are critical for stable learning.  
- Log-space computation and smoothing are essential for numerical stability in sequence modeling.

---


## 🧪 Tools & Libraries

- **Python 3.10+**
- **NumPy**
- **Pandas**
- **Matplotlib**
- **tqdm** for progress visualization

---


