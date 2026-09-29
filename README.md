# Exact Domination of Lecture Timings

## Problem

A supervisor wants to select some courses so that the TAs of the selected courses can supervise all lectures. Each course has one lecture per day, and lectures may wrap past midnight.

Given `n` lectures, select a subset such that **every lecture overlaps with exactly one lecture from the subset** (a selected lecture overlaps itself). If no such subset exists, report that.

In graph terms, lectures are arcs on a 24-hour circle, and the task is to find an exact dominating set (perfect code) in a circular-arc graph.

## Assumptions

- Each lecture is a half-open interval `[start, end)` on a 1440-minute circle.
- Back-to-back lectures (one ends at 10:00, the next starts at 10:00) **do not** overlap.
- If `start == end`, the lecture is treated as a 1-minute lecture.

## Input Format

```
n
HH:MM HH:MM
HH:MM HH:MM
...
```

The first line is the number of lectures `n`. Each of the next `n` lines gives the start and end time of one lecture in 24-hour `HH:MM` format. If the end time is earlier than the start time, the lecture crosses midnight.

## Output Format

- `Selected lectures: i j k ...` with 1-based lecture numbers in increasing order, or
- `No valid subset exists.`

If several valid subsets exist, any one of them is printed.

## Example

Input:

```
4
08:00 10:00
09:00 11:00
10:00 12:00
11:00 13:00
```

Output:

```
Selected lectures: 1 4
```

## How to Compile and Run

Requires a C++ compiler with C++11 or later (e.g. g++ from MinGW-w64 / MSYS2).

```
g++ src/exact_domination.cpp -o exact_domination
./exact_domination < tests/input.txt
```

Replace `tests/input.txt` with any input file in the format above. On Windows PowerShell, use `./exact_domination.exe < tests\input.txt` if needed. You can also run the program without redirection and type the input by hand.

Note: run the compile and run commands as two separate commands (or join them with `;` in PowerShell).

## Algorithm Summary

1. Convert times to minutes and build an `overlap[i][j]` table.
2. **Single lecture case:** if one lecture overlaps every lecture, output it.
3. **General case:** the selected lectures are pairwise non-overlapping and appear in a cyclic order around the day. For consecutive selected lectures `a` then `b`:
   - no lecture may overlap both `a` and `b` (it would be covered twice), and
   - no lecture may lie entirely in the gap between them (it would be uncovered).

   Call this relation `canFollow[a][b]`.
4. Find a chain of lectures that goes once around the circle and closes back on itself, using `canFollow` for every step. Some selected lecture must overlap lecture 1, so each lecture overlapping lecture 1 is tried as the starting point, and a dynamic program over lectures sorted by start time searches for a chain.
5. If no chain is found, print `No valid subset exists.`

The full correctness proof and analysis are in the report.

## Complexity

- **Time:** O(n³)
- **Space:** O(n²)

## Test Cases

`testcases.txt` contains 11 test cases with expected outputs, covering:

- a single lecture
- one lecture overlapping all others
- identical lectures
- fully disjoint lectures
- separate groups of lectures
- a chain with a unique answer
- touching lectures (end time equals next start time)
- lectures crossing midnight
- a ring of 6 lectures (solution wraps around the day)
- rings of 5 and 4 lectures (no solution)

Each case is labelled with `INPUT` and `EXPECTED`. Copy the lines of one input into its own file to run it.

## Repository Structure

```
.
├── README.md
├── src/
│   ├── exact_domination.cpp    efficient algorithm (main solution)
├── tests/
│   └── all_testcases.txt       labelled test cases with expected outputs
└── report/
    ├── report.tex
    └── report.pdf
```

## Report

The project report (LaTeX source and PDF) is in the `report/` folder.

## Author

Devesh Kumar (bmat2319)
Arjina Jana (bmat2312)
