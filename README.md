# Exact Domination of Lecture Timings

Given `n` daily lectures (start and end times, possibly crossing midnight), pick a
subset so that **every lecture overlaps exactly one chosen lecture** (a chosen
lecture overlaps itself). The program prints such a subset, or says none exists.

## Assumptions

- Lectures are half-open: if one ends at 10:00 and another starts at 10:00, they
  do **not** overlap.
- A lecture with the same start and end time is treated as 1 minute long.

## Input and output

Input: `n`, then `n` lines of `HH:MM HH:MM` (start, end).

```
4
08:00 10:00
09:00 11:00
10:00 12:00
11:00 13:00
```

Output: the chosen lecture numbers, or `No valid subset exists.`

```
Selected lectures: 1 4
```

If several answers are valid, any one is printed.

## Build and run

```
g++ src/exact_domination.cpp -o exact_domination
./exact_domination < input.txt
```

(On Windows PowerShell, use `.\exact_domination.exe < input.txt`.)

## How it works, briefly

Chosen lectures can't overlap each other, so they sit around the 24-hour circle
like beads on a necklace. The code checks which lecture can come right after
which, then searches for a chain that goes once around the circle and closes up.
Time: O(n^3), space: O(n^2). The full explanation and proof are in the report.

## Files

- `src/exact_domination.cpp`: the efficient solution
- `tests/testcases.txt`: 11 test cases with expected outputs (copy one input
  into its own file to run it)
- `report/report.tex`, `report/report.pdf`: the project report

## Author

Devesh Kumar (bmat2319)
Arjina Jana (bmat2312)
