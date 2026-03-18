# 🎮 Game Glitch Investigator: The Impossible Guesser

## 🚨 The Situation

You asked an AI to build a simple "Number Guessing Game" using Streamlit.
It wrote the code, ran away, and now the game is unplayable.

- You can't win.
- The hints lie to you.
- The secret number seems to have commitment issues.

## 🛠️ Setup

1. Install dependencies: `pip install -r requirements.txt`
2. Run the fixed app: `python -m streamlit run app.py`

## 🕵️‍♂️ Your Mission

1. **Play the game.** Open the "Developer Debug Info" tab in the app to see the secret number. Try to win.
2. **Find the State Bug.** Why does the secret number change every time you click "Submit"? Ask ChatGPT: *"How do I keep a variable from resetting in Streamlit when I click a button?"*
3. **Fix the Logic.** The hints ("Higher/Lower") are wrong. Fix them.
4. **Refactor & Test.** - Move the logic into `logic_utils.py`.
   - Run `pytest` in your terminal.
   - Keep fixing until all tests pass!

## 📝 Document Your Experience

- [x] **Purpose:** This is a number guessing game built with Streamlit where the player tries to guess a secret number within a limited number of attempts. The game gives hints after each guess to guide the player toward the answer.

- [x] **Bugs Found:**
  1. **Hints were inverted** — `check_guess` returned "Go HIGHER!" when the guess was too high and "Go LOWER!" when it was too low, making the hints completely misleading.
  2. **Secret number alternated types** — Every even attempt, `app.py` cast the secret number to a `str`, which broke comparisons and made it impossible to win on even-numbered attempts.
  3. **New Game didn't fully reset** — Clicking "New Game" only reset the secret and attempt count, leaving the score, history, and game status stuck from the previous game.
  4. **Score was inconsistent** — `update_score` added +5 points on even-numbered wrong guesses instead of always subtracting, causing the score to jump unpredictably.

- [x] **Fixes Applied:**
  - Swapped the hint messages in `check_guess` in `logic_utils.py`
  - Removed the `str()` cast on the secret number in `app.py`
  - Added full reset of `score`, `status`, and `history` in the New Game handler
  - Simplified `update_score` to always subtract 5 for any wrong guess
  - Refactored all game logic out of `app.py` and into `logic_utils.py`

## 📸 Demo

### ✅ Pytest Results — All 3 Tests Passing
<img width="1277" height="268" alt="Screenshot 2026-03-17 at 11 14 50 PM" src="https://github.com/user-attachments/assets/c7916d32-6c6b-4f34-9860-c0e6b7c034bb" />




### 🎮 Fixed Game Screenshot
- [ ] [Insert a screenshot of your fixed, winning game here]
<img width="1896" height="930" alt="Screenshot 2026-03-17 at 11 22 22 PM" src="https://github.com/user-attachments/assets/940b1263-6f88-4997-bd3a-d440dbed5b2d" />

      

## 🚀 Stretch Features

- [ ] [If you choose to complete Challenge 4, insert a screenshot of your Enhanced Game UI here]
