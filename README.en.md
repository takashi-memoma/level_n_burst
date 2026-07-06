# Level f(n) Burst

[日本語版 README はこちら / Japanese README](./README.md)

A puzzle game where you chant a sequence of numbers, a grimoire (the **Berlekamp–Massey algorithm**) uncovers the shortest linear recurrence behind it, and the spell strikes every enemy whose level lies on the recurrence's trajectory. It is a homage to a certain RPG's "blue magic that targets all enemies by a level condition" — except that instead of "multiples of n," you aim with a recurrence of your own design.

## Rules

- Chant comma-separated integers. The shortest linear recurrence is recovered automatically, and its **order (length) is the MP cost** of the spell. Unstructured sequences cost more; finding hidden structure makes your spells cheap.
- The spell **keeps flying along the recurrence** after your chant ends. Chant `5, 10, 15, 20` and the next stop is 25 — if an ally stands there, they get hit and healing costs 2 MP.
- If the next term becomes a **fraction**, the spell dissipates; if it exceeds **level 99**, it flies off the board. Advanced play exploits this: bend an arithmetic run like `3, 8, 13, 21` so the coefficients turn fractional and the trajectory self-destructs before reaching an ally.
- If you chant fewer than twice the order, multiple minimal recurrences fit your sequence (an **ambiguous** badge appears). The grimoire's output is still deterministic, and the preview shows exactly what will fly. Chanting at least 2 × order terms pins the trajectory down uniquely (a **locked** badge).
- Defeat every enemy within the MP budget to clear the stage; match the reference MP for the top rank.

## The algorithm inside

Berlekamp–Massey referees everything.

- **MP cost**: the minimal-order recurrence is recovered exactly over the rationals (BigInt fractions). Since any N-term sequence fits some recurrence of order about ⌈N/2⌉, "list every enemy level and one-shot the board" is always expensive — the game balance falls out of the theorem itself.
- **Trajectory extension**: the recovered recurrence extends the sequence, stopping when a term turns non-integral (dissipate), leaves the 1–99 board (out of bounds), or hits a safety cap for periodic sequences.
- **Uniqueness badge**: the classical theorem "the minimal recurrence is unique iff N ≥ 2L" is rendered directly as UI. For example `1, 2, 4, 7` has N = 4, L = 3 and infinitely many minimal solutions (second differences +1, tribonacci, and more), so its badge turns amber.

## Development

- One `index.html` (HTML + CSS + vanilla JS). No external dependencies, no build.
- All arithmetic uses exact BigInt fractions, so there are no floating-point misjudgements.
