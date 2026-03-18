# 💭 Reflection: Game Glitch Investigator

Answer each question in 3 to 5 sentences. Be specific and honest about what actually happened while you worked. This is about your process, not trying to sound perfect.

## 1. What was broken when you started?

When I first ran the game, it looked like a normal number guessing game but quickly showed strange behavior as soon as I started guessing. The hints were completely backwards — when my guess was higher than the secret number, the game told me to go higher instead of lower, making it impossible to use the hints to win. Clicking "New Game" only reset the secret number and attempt count, but the old guess history, score, and game status carried over, so the game never truly restarted. The score also behaved inconsistently, sometimes going up by 5 points on wrong guesses instead of always going down, because the code added points on even-numbered attempts.

---

## 2. How did you use AI as a teammate?

I used Claude and GitHub Copilot as AI tools on this project to help investigate and explain the buggy code. One correct suggestion the AI gave was identifying that the `check_guess` function had its return messages swapped — it pointed out that `"Go HIGHER!"` was returned when `guess > secret`, which is logically backwards, and after verifying this against the game behavior and fixing the messages, the hints worked correctly. One misleading suggestion was when the AI initially focused only on the hint messages as the sole bug, missing the deeper issue in `app.py` where the secret number was being cast to a string (`str(st.session_state.secret)`) on every even attempt — this caused the comparison to break entirely, and I had to trace through the code manually to find it.

---

## 3. Debugging and testing your fixes

I decided a bug was truly fixed by first running the app manually and confirming the behavior matched expectations — for example, guessing 60 when the secret was 30 should now show "Go LOWER!" I also used `pytest` to run the tests in `test_game_logic.py`, which tested `check_guess` directly with known inputs like `check_guess(60, 50)` expecting `"Too High"`. Before my fix, the test for `test_guess_too_high` failed because the function returned the wrong message; after moving the corrected logic into `logic_utils.py`, all three tests passed. AI helped me understand that the test was failing not because of the message text but because of the comparison logic itself, which pointed me back to the root cause.

---

## 4. What did you learn about Streamlit and state?

Streamlit reruns the entire Python script from top to bottom every single time a user does anything — clicks a button, types in a box, or changes a dropdown. This means any normal Python variable gets reset to its starting value on every rerun, which is why the secret number, score, and history kept disappearing or behaving unpredictably. Streamlit's `session_state` solves this by acting like a persistent memory that survives across reruns — values stored there stick around between interactions. The "New Game" bug happened because only some `session_state` keys were reset when the button was clicked, leaving `status`, `score`, and `history` stuck at their old values from the previous game.

---

## 5. Looking ahead: your developer habits

One habit I want to carry forward is running the existing tests first before making any changes, so I can confirm which tests fail and use that as a map of what is broken — this made it much clearer which functions needed to be fixed and in what order. Next time I work with AI on a debugging task, I would share all the relevant files at once from the start rather than one file at a time, because some bugs (like the `str()` casting issue) only make sense when you can see how `app.py` and `logic_utils.py` interact with each other. This project changed how I think about AI-generated code: I now treat it as a starting draft that can contain subtle, hard-to-spot logic errors that only reveal themselves when you actually run the code and test edge cases.
