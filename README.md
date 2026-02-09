# 🏀 NBA Play‑By‑Play Stat Engine
Reconstructing full box scores and Game Score from raw NBA event logs


Overview
This project ingests raw NBA play‑by‑play data and rebuilds complete single‑game statlines directly from the event model. Instead of relying on pre‑cleaned box score files, the pipeline decodes the underlying structure of the NBA’s event feed to compute points, rebounds, assists, steals, blocks, and Game Score for every player in every game.

The goal is to show how real stat sites (like Basketball‑Reference) transform low‑level event logs into the box scores fans recognize.

Why This Matters
NBA play‑by‑play files look simple on the surface, but the real information lives in the structural fields—not the human‑readable text. This project demonstrates how to reverse‑engineer that structure:

EVENTMSGTYPE defines the type of action

PLAYER1_ID, PLAYER2_ID, and PLAYER3_ID encode the roles

rebounds, assists, steals, and blocks are hidden in specific ID patterns

the text description is unreliable and not used for stat extraction

By decoding these rules, the script generalizes across seasons without modification.

Features
Parses raw NBA play‑by‑play CSVs

Extracts all major box score stats from structural fields

Computes Game Score for every player performance

Produces sortable tables of top single‑game performances

Works across seasons with no code changes (only the filename changes)

Example Output
Top single‑game performances (2000–01 season):

Code
   GAME_ID  PLAYER_ID       PLAYER_NAME   PTS   REB   AST  STL  BLK  GAME_SCORE
0  20000471        185      Chris Webber  51.0  26.0   5.0  3.0  2.0        69.3
1  20001068        711  Jerry Stackhouse  57.0   4.0   5.0  1.0  0.0        63.1
2  20000557        952    Antoine Walker  47.0   5.0  13.0  4.0  1.0        62.8
3  20000267       1712    Antawn Jamison  51.0  13.0   5.0  2.0  1.0        62.4
4  20000247       1712    Antawn Jamison  51.0  14.0   2.0  3.0  2.0        62.4
These results match real historical performances, confirming the correctness of the event‑model decoding.

How It Works
Event Model Interpretation
Made shots → EVENTMSGTYPE == 1

Points come from shot type

Assists come from PLAYER2_ID

Missed shots → EVENTMSGTYPE == 2

Blocks come from PLAYER3_ID

Rebounds → EVENTMSGTYPE == 4

Rebounder is always PLAYER1_ID

Steals → EVENTMSGTYPE == 5

Stealer is PLAYER2_ID

Turnovers → EVENTMSGTYPE == 5

Turnover committed by PLAYER1_ID

The script aggregates these events into per‑player totals.

Game Score
Game Score is computed using the standard John Hollinger formula, combining scoring, efficiency, and defensive contributions into a single metric.

Usage
Place any NBA play‑by‑play CSV in the project directory

Update the filename in the script

Run the pipeline to generate full statlines and Game Scores

The logic works across seasons because the NBA’s event model is stable.

Future Extensions
Store results in a relational database

Add advanced stats (TS%, ORB%, AST%, STL%, BLK%)

Build per‑season leaderboards

Construct full box scores per game

Add lineup and on/off analysis
