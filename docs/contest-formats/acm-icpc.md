# ICPC Contest Format & Scoring Rules

FuraOJ provides native support for International Collegiate Programming Contest (ICPC) scoring and leaderboard semantics.

## Scoring Rules

1. **Primary Ranking Criteria:** Total number of distinct problems solved (`AC`).
2. **Tie-Breaking Criteria:** Lower total penalty time (in minutes).
3. **Penalty Calculation:**
   $$\text{Penalty} = \sum_{\text{solved problems}} (\text{Elapsed Minutes} + 20 \times (\text{Rejected Attempts}))$$
   - Failed submissions on problems that are never solved do not contribute to penalty time.
4. **Scoreboard Freeze:**
   - Contests can specify a frozen interval (typically the final 60 minutes).
   - During the freeze period, subsequent submissions display as pending without altering public standings.
