# PPA table (fill in the empty cells)

One row per run. "Yosys cells" is our own synthesis count, for orientation only. Reference rows: your earlier A1 / B runs (latch study, ORFS, sky130hd, 10 ns).

| run | design | LLM modules | Yosys cells | area um2 | fmax MHz | total power mW | setup WNS / TNS ns | unconstrained endpoints (check_setup) | notes (die area used, DRC, errors) |
|---|---|---|---|---|---|---|---|---|---|
| 1 | 2x2/C0_golden | 0 of 34 | 119,720 |  |  |  |  |  |  |
| 2 | 2x2/P1_oneshot | 9 of 34 | 118,840 |  |  |  |  |  |  |
| 3 | 2x2/P2_loop_stitch | 14 of 34 | 119,280 |  |  |  |  |  |  |
| 4 | 3x3/C0_golden | 0 of 34 | 269,370 |  |  |  |  |  |  |
| 5 | 3x3/P1_oneshot | 9 of 34 | 267,390 |  |  |  |  |  |  |
| 6 | 3x3/P2_loop_stitch | 14 of 34 | 268,380 |  |  |  |  |  |  |
| 7 | ni/C0_golden | 0 of 9 | 1,799 |  |  |  |  |  |  |
| 8 | ni/P1_oneshot_loop_stitch | 2 of 9 | 1,767 |  |  |  |  |  |  |

Reference (already measured, latch study):

| design | area um2 | fmax MHz | total power mW | setup WNS / TNS ns |
|---|---|---|---|---|
| A1 / 2x2 | 780,685 | 106.84 | 138.3 | not measured |
| B / 2x2 | 808,319 | 108.0 | 151.1 | not measured |
| A1 / 3x3 | 2,235,731 | 99.2363 | 392.2 | -0.0769544 / -0.322059 |
| B / 3x3 | 2,312,740 | 98.0925 | 429.1 | -0.194461 / -0.718273 |
