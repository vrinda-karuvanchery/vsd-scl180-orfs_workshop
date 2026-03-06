# Phase 1 - ORFS Execution in GitHub Codespaces

## Scope
Run ORFS in GitHub Codespaces and execute flow progress through floorplan.

## Completed Work
1. Forked upstream repository and launched Codespaces.
2. Verified workspace structure in root (`README.md`, `images`, `orfs`).
3. Verified OpenROAD availability and version in Codespaces.
4. Prepared SCL180 platform files (LEF/GDS/LIB and liberty filtering).
5. Started flow run and reached floorplan milestone.

## Commands Used
```bash
openroad -version
ls -ltr

cp scl180/stdcell/fs120/6M1L/lef/scl18fs120_tech.lef \
  orfs/flow/platforms/scl180fs120/lef/
cp scl180/stdcell/fs120/6M1L/lef/scl18fs120_std.lef \
  orfs/flow/platforms/scl180fs120/lef/scl18fs120_merged.lef
cp scl180/stdcell/fs120/6M1L/gds/scl18fs120.gds \
  orfs/flow/platforms/scl180fs120/gds/
cp scl180/stdcell/fs120/6M1L/liberty/lib_flow_ss/tsl18fs120_scl_ss.lib \
  orfs/flow/platforms/scl180fs120/lib/

export SCL_LIB=orfs/flow/platforms/scl180fs120/lib/tsl18fs120_scl_ss.lib
awk '
BEGIN{skip=0}
/^[ \t]*cell\(/ {
  skip=0
  if ($0 ~ /cell\(srlab(1|2|4)\)/) skip=1
}
{ if (!skip) print }
' "$SCL_LIB" \
> orfs/flow/platforms/scl180fs120/lib/tsl18fs120_scl_ss.nosrlab.lib

cd orfs/flow
make clean_all
make
```

## Evidence Images
### Environment and setup
![Extract SCL PDK](../../images/1_StepsToExtractSCLPDK.png)
![Copy technology LEF](../../images/2_copyTechLef.png)
![Copy standard-cell LEF](../../images/3_copyStdCellLef.png)
![Verify LEFs in platform](../../images/4_VerifySCLLefPresentInORFSPlatform.png)
![Copy GDS](../../images/5_CopySCLGDSAndVerifyOnORFSPlatform.png)
![Copy SS liberty](../../images/6_VeryImpCopySCL_SS_LIBAndVerifyOnORFSPlatform.png)
![Filter liberty cells](../../images/7_VeryImportant_EditLibAndVerifyOnORFSPlatform.png)

### Flow run and GUI
![Start flow make clean and make](../../images/8_startFlow_make_clean_all_and_make.png)
![Run completion and layout command](../../images/9_ProofOfRunCompletionAndCommandToObserveLayout.png)
![Open ports for noVNC](../../images/10_OpenPortsOnGitHubCodeSpaceToAccessVNC.png)
![Open VNC lite](../../images/11_Open_VNC_Lite.png)
![Observe layout](../../images/12_ObserveLayout.png)

## Additional Screenshot Evidence (Provided in Discussion)
These screenshots were shared and used as relevant evidence:
1. `openroad -version` output visible in Codespaces terminal.
2. Root `ls -ltr` output showing repository directories and files.
3. ORFS directory `ls -ltr` output showing build and setup scripts.
4. `results/final` directory listing showing generated outputs (`lef`, `def`, `gds`, `verilog`, `spef`, `sdf`, `lib`).
5. Design config view showing active design config lines in flow files.

## Current Phase 1 Status
- Codespaces setup: Completed.
- Tool invocation checks: Completed.
- Flow execution: Completed up to floorplan.
- Remaining in phase: collect explicit floorplan log snippet and continue to placement/CTS/routing/final reports.

## Pending Additions
```bash
cd orfs/flow
find logs -type f | grep -i floorplan
# less logs/<platform>/<design>/<variant>/2_1_floorplan.log
```
