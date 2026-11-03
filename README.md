# STITCH ORFS runs: NoC routers with LLM-written modules

Our hand-written NoC router (from the RISC-V MPSoC) with some modules rewritten by an LLM (Qwen2.5-Coder 32B)
and the rest kept as the original (golden) RTL. We need their PPA from ORFS, measured exactly like your A1 and B
runs in the latch study.

## The 8 runs

| run | folder | top | LLM-written modules |
|---|---|---|---|
| 1 | `2x2/C0_golden` | top_2x2_router | 0 of 34 |
| 2 | `2x2/P1_oneshot` | top_2x2_router | 9 of 34 |
| 3 | `2x2/P2_loop_stitch` | top_2x2_router | 14 of 34 |
| 4 | `3x3/C0_golden` | top_3x3_router | 0 of 34 |
| 5 | `3x3/P1_oneshot` | top_3x3_router | 9 of 34 |
| 6 | `3x3/P2_loop_stitch` | top_3x3_router | 14 of 34 |
| 7 | `ni/C0_golden` | ni_pair | 0 of 9 |
| 8 | `ni/P1_oneshot_loop_stitch` | ni_pair | 2 of 9 |

- **C0**: every module original. In 2x2 and 3x3 it must reproduce your B numbers (`PPA_TABLE.md`). Run it first;
  if it does not match, stop and tell us.
- **P1**: the modules the LLM got fully right in one try, original for the rest.
- **P2**: the same after up to 6 retries. The simulation-only loop and STITCH produced the same design, so one run
  covers both.
- **NI**: one-shot, loop and STITCH all produced the same design, so there is one LLM run.

Every folder has the same file names because every design is the complete router. Only the content of the
LLM-written modules differs. `SOURCES.csv` in each folder says which modules came from the LLM.

## How to run one design

1. Copy the folder into ORFS (for example `flow/designs/sky130hd/<folder name>/`).
2. Make `config.mk` from `config.mk.template`. DESIGN_NAME, DESIGN_NICKNAME and VERILOG_FILES are filled in. Copy
   every other line from your A1 config.mk: sky130hd, 10 ns clock, CORE_UTILIZATION, flow settings.
3. SDC:
   - 2x2 and 3x3: your A1 `constraint.sdc`, unchanged. The clock port is `clk`, as in A1.
   - NI: `constraint.sdc` in the folder. It has two clocks (`rd_clk`, `wr_clk`, 10 ns each, asynchronous). This is
     the only SDC change versus A1.
4. Floorplan: keep A1's CORE_UTILIZATION. If your A1 config.mk fixes DIE_AREA / CORE_AREA instead:
   - 3x3: scale each side by 1.5.
   - NI (tiny, 120 I/O pins): use the smallest die that fits the pins.

   Use the same floorplan for every run of a study.
5. Run the flow. Then run `check_setup -verbose` in OpenSTA.

## What to record (`PPA_TABLE.md`)

- Area.
- Fmax.
- Total power.
- Setup WNS / TNS at the 10 ns clock.
- Number of unconstrained endpoints (from `check_setup`).
- Notes: die area used, DRC, any errors.

## What to send back

- The filled `PPA_TABLE.md`.
- The ORFS reports and logs of each run, zipped.
- Your A1 `config.mk` and `constraint.sdc`.

## Notes

- No Vivado was used anywhere. Simulation used Icarus Verilog, lint used Verilator, and synthesis checks used Yosys.
  ORFS is the only physical-design step.
- Every LLM module in P1 and P2 passed its own checks: compile, testbench, sign-off testbench, no latch, no
  combinational loop. One known quirk: P2's encoder gives a different code from the original when more than one
  input is active.
- Source: [SOURCE_MAP.md](SOURCE_MAP.md) (copied byte for byte from Karthik-Flash/STITCH).
