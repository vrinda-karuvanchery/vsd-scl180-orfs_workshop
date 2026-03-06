# VSD SCL180 ORFS Workshop - Week 2 Documentation

Fork: [vrinda-karuvanchery/vsd-scl180-orfs_workshop](https://github.com/vrinda-karuvanchery/vsd-scl180-orfs_workshop)  
Upstream: [vsdip/vsd-scl180-orfs](https://github.com/vsdip/vsd-scl180-orfs)

## Week 2 Objective
Build independent RTL-to-GDS execution capability using ORFS/OpenROAD across cloud and local environments.

Reference document used:  
[Week-2 Google Doc](https://docs.google.com/document/d/1ZlxIZmNuuCeij1zmFxtIdxzhbBbAIVUlQXhYC893fqI/edit?tab=t.0)

## Current Progress
Status is updated only up to floorplan, as completed so far.

| Phase | Scope | Status |
|---|---|---|
| Phase 1 | ORFS in GitHub Codespaces | In progress (completed up to floorplan) |
| Phase 2 | Toolchain mapping from devcontainer | Pending |
| Phase 3 | Local ORFS + OpenROAD install | Pending |
| Phase 4 | Local rerun and cloud vs local comparison | Pending |
| Phase 5 | Unix/debugging evidence | Pending |

## Work Done So Far (Up To Floorplan)

### Phase 1.1 - Repository and Codespaces Setup
Completed steps:
1. Forked the upstream repository.
2. Launched GitHub Codespaces.
3. Completed initial environment setup.

Environment verification commands used:
```bash
openroad -version
yosys -V
python3 --version
make --version
```

### Phase 1.2 - SCL180 Platform Preparation
The following PDK integration steps are completed in the flow platform directory:

```bash
cp scl180/stdcell/fs120/6M1L/lef/scl18fs120_tech.lef \
   orfs/flow/platforms/scl180fs120/lef/

cp scl180/stdcell/fs120/6M1L/lef/scl18fs120_std.lef \
   orfs/flow/platforms/scl180fs120/lef/scl18fs120_merged.lef

cp scl180/stdcell/fs120/6M1L/gds/scl18fs120.gds \
   orfs/flow/platforms/scl180fs120/gds/

cp scl180/stdcell/fs120/6M1L/liberty/lib_flow_ss/tsl18fs120_scl_ss.lib \
   orfs/flow/platforms/scl180fs120/lib/
```

Liberty cleanup step used:
```bash
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
```

### Flow Execution (Until Floorplan)
Commands used:
```bash
cd orfs/flow
make clean_all
make
```

Current milestone reached: `floorplan` stage.

## Evidence Collected So Far

### Setup and PDK integration
- ![Extract SCL PDK](images/1_StepsToExtractSCLPDK.png)
- ![Copy technology LEF](images/2_copyTechLef.png)
- ![Copy standard-cell LEF](images/3_copyStdCellLef.png)
- ![Verify LEFs in platform](images/4_VerifySCLLefPresentInORFSPlatform.png)
- ![Copy GDS](images/5_CopySCLGDSAndVerifyOnORFSPlatform.png)
- ![Copy liberty](images/6_VeryImpCopySCL_SS_LIBAndVerifyOnORFSPlatform.png)
- ![Edit liberty](images/7_VeryImportant_EditLibAndVerifyOnORFSPlatform.png)
- ![Start flow run](images/8_startFlow_make_clean_all_and_make.png)

### Pending evidence to add after next run
- Synthesis completion snippet
- Floorplan log snippet (current stage proof)
- Placement completion proof
- CTS/routing/GDS/timing evidence

## Useful Log Commands For Next Update
```bash
cd orfs/flow
find logs -type f | grep -i floorplan

# Example once log is present:
# less logs/<platform>/<design>/<variant>/2_1_floorplan.log
```

## Next Steps
1. Capture and add explicit floorplan log snippet in this README.
2. Continue run to placement, CTS, routing, and final GDS.
3. Fill Phase 2 toolchain mapping table from `.devcontainer/Dockerfile` and `.devcontainer/install-openroad.sh`.
4. Add cloud runtime and WNS/TNS summary after full run.

## Attribution
This repository is based on [vsdip/vsd-scl180-orfs](https://github.com/vsdip/vsd-scl180-orfs).  
Original framework and collateral are from the upstream project; this fork documents my own execution and learning notes.
