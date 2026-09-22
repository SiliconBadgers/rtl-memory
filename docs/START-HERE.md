# Memory Control starting material

September 22, 2026. Initial investigations for team discussion; no personal assignments or deadlines.

## Shared starting points

- [Editable architecture diagram](https://github.com/SiliconBadgers/architecture/blob/main/docs/accelerator-diagram.md) and [candidate boundaries](https://github.com/SiliconBadgers/architecture/blob/main/contracts/accelerator-boundaries.md).
- [Workload cases and source shapes](https://github.com/SiliconBadgers/architecture/blob/main/docs/workload-cases.md).
- [Measured llama.cpp report](https://github.com/SiliconBadgers/software/blob/main/experiments/llama-cpp/2026-09-22/REPORT.md) and [reproduction procedure](https://github.com/SiliconBadgers/software/blob/main/experiments/llama-cpp/2026-09-22/README.md).
- [Parallel team investigations](https://github.com/SiliconBadgers/planning/blob/main/docs/team-start.md).

The diagram and engine split are proposals. Start from available shapes and
reference cases now; use explicit parameters or stubs where decisions remain
open. Software's broader profiling study is not a prerequisite. Preserve the
source revision, assumptions, commands and limits of each result. Members and
leads can choose a different investigation that resolves a relevant uncertainty.


## First useful output

Build a parameterized storage/traffic model and a candidate bank/buffer diagram.
Start from the shared matrix cases and the recorded operation shapes. Separate
weights/scales, activations, partial sums, KV cache, recurrent state and
convolution history. Count their lifetimes and simultaneous accesses.

## Procedure

1. Select a prefill microbatch and single-token decode case. State numerical formats explicitly; the source GGUF is mixed-format.
2. For candidate tiles, calculate resident bytes, fetched/stored bytes and spill bytes. Explain reuse and whether weights are already resident.
3. Sketch banks, ports, arbitration and ownership. Test a schedule where load, compute and store overlap; identify shared-port conflicts.
4. Sweep tile size, bank/port count and external bandwidth as parameters. Give a bandwidth-limited time estimate alongside the compute estimate.
5. Check macro/port/latency feasibility with Physical Design. Keep F2 external-memory paths separate from first-chip local SRAM staging.

Save inputs, formulas/script, output tables and assumptions in
`experiments/<study>/`. Logical tensor bytes are not measured DRAM traffic;
include packing, scales, padding, repeated fetches and transaction overhead
where relevant. Do not claim a bandwidth measurement without a microbenchmark.

## First behavioral cases

Use a reference memory model to exercise competing reads/writes, backpressure,
tail tiles, delayed responses and a fault with traffic outstanding. Specify when
a buffer can be released. A timeout does not cancel an accepted DMA transaction.

## Useful exchange

Send Compute a feasible operand-rate/port envelope and Control a transfer-latency
model. Receive state lifetimes from Compute 2 and Software incrementally. Missing
macro details remain explicit bounds; they do not prevent the traffic study.
`make test` currently reports an unimplemented scaffold, not a passing memory test.
