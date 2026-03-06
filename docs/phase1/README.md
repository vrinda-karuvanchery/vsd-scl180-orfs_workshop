# Phase 1 - ORFS Execution in GitHub Codespaces

## Scope
Run ORFS in GitHub Codespaces and execute flow progress through floorplan.

## Completed Work
1. Forked upstream repository and launched Codespaces.
2. Verified workspace structure in root (`README.md`, `images`, `orfs`).
3. Verified OpenROAD availability and version in Codespaces.
4. Prepared SCL180 platform files (LEF/GDS/LIB and liberty filtering).
5. Started flow run and reached floorplan milestone.

## Screenshot Evidence
### Workspace and setup checks
![Repository layout](../../my_images/repository_layout.png)
Shows the top-level workspace structure with `README.md`, `images`, and `orfs`.
![ORFS repository listing](../../my_images/orfs_repository.png)
Shows ORFS directory contents, including flow scripts and setup/build files.
![OpenROAD version](../../my_images/version_openroad.png)
Confirms OpenROAD is installed and runnable in the Codespaces environment.
![Yosys version](../../my_images/Yosys_version.png)
Confirms Yosys is available for synthesis in the same environment.
![Python version](../../my_images/python_version.png)
Confirms Python runtime version used by ORFS utility scripts.
![Make version](../../my_images/make-version.png)
Confirms GNU Make version used to orchestrate flow stages.

### Synthesis and floorplan stage evidence
![Synthesis log](../../my_images/synthesis_log.png)
Shows `1_synth.log` output, proving synthesis completed and reports were generated.
![Floorplan timing report](../../my_images/timing_floorplan.png)
Shows floorplan timing metrics including TNS, WNS, worst slack, and derived fmax.
![Floorplan power report](../../my_images/power_floorplan.png)
Shows floorplan power breakdown across sequential and combinational logic.
![Floorplan design area and checks](../../my_images/floorpla_log_design_area.png)
Shows floorplan checks, warnings, and design area/utilization summary.
![Chip area summary](../../my_images/chip_area_total_module_riscv.png)
Shows synthesized chip area of top module `riscv` and sequential contribution.
![Synthesized netlist snippet](../../my_images/riscv_synthesized_netlist.png)
Shows a mapped netlist excerpt using sky130 standard cells after synthesis.

## Extracted Values From Uploaded Screenshots
### Environment checks
- `openroad -version`: `v2.0-28075-g0f99689f45`
- `yosys -V`: `Yosys 0.58+94`
- `python3 --version`: `Python 3.10.12`
- `make --version`: `GNU Make 4.3`

### Synthesis and floorplan evidence
- Synthesis stage log (`1_synth.log`) is present and completed.
- Floorplan timing snapshot:
  - `tns max = -1236.72`
  - `wns max = -2.02`
  - `worst slack max = -2.02`
  - `clk period_min = 8.02`, `fmax = 124.62`
- Floorplan power snapshot:
  - Total power: `1.21e-02 W`
  - Sequential share: `75.1%`
  - Combinational share: `24.9%`
- Floorplan/check setup snapshot:
  - Warning: `65 input ports missing set_input_delay`
  - Warning: `995 unconstrained endpoints`
  - Instances in design: `4644`
  - Design area: `61496 um^2`
  - Utilization: `46%`

## Current Phase 1 Status
- Codespaces setup: Completed.
- Tool invocation checks: Completed.
- Flow execution: Completed up to floorplan with timing/power/area evidence captured from screenshots.
- Remaining in phase: collect explicit floorplan log snippet and continue to placement/CTS/routing/final reports.

## Pending Additions
```bash
cd orfs/flow
find logs -type f | grep -i floorplan
# less logs/<platform>/<design>/<variant>/2_1_floorplan.log
```
