# List of steps

## Linting
+ ``` Verilator.Lint ```
+ ``` Checker.LintTimingConstructs ```
+ ``` Checker.LintErrors ```
+ ```Checker.LintWarnings```
## Synthesis
+ ```Yosys.JsonHeader```
+ ```Yosys.Synthesis```
+ ```Checker.YosysUnmappedCells```
+ ```Checker.YosysSynthChecks```
+ ```Checker.NetlistAssignStatements```
## Floorplaning
+ ```OpenROAD.CheckSDCFiles```
+ ```OpenROAD.CheckMacroInstances```
+ ```OpenROAD.STAPrePNR```
+ ```OpenROAD.Floorplan```
+ ```OpenROAD.DumpRCValues```
+ ```Odb.CheckMacroAntennaProperties```
+ ```Odb.SetPowerConnections```
+ ```Odb.ManualMacroPlacement```
+ ```OpenROAD.CutRows```
+ ```OpenROAD.TapEndcapInsertion```
+ ```Odb.AddPDNObstructions```
+ ```OpenROAD.GeneratePDN```
+ ```Odb.RemovePDNObstructions```
+ ```Odb.AddRoutingObstructions```
## Placement
+ ```OpenROAD.GlobalPlacementSkipIO```
+ ```OpenROAD.IOPlacement```
+ ```Odb.CustomIOPlacement```
+ ```Odb.ApplyDEFTemplate```
+ ```OpenROAD.GlobalPlacement```
+ ```Odb.WriteVerilogHeader```
+ ```Checker.PowerGridViolations```
+ ```OpenROAD.STAMidPNR```
+ ```OpenROAD.RepairDesignPostGPL```
+ ```Odb.ManualGlobalPlacement```
+ ```OpenROAD.DetailedPlacement```
## Clock Tree Synthesis 
+ ```OpenROAD.CTS```
+ ```OpenROAD.STAMidPNR-1```
+ ```OpenROAD.ResizerTimingPostCTS```
+ ```OpenROAD.STAMidPNR-2```
## Routing 
+ ```OpenROAD.GlobalRouting```
+ ```OpenROAD.CheckAntennas```
+ ```Odb.DiodesOnPorts```
+ ```OpenROAD.RepairAntennas```
+ ```OpenROAD.DiodeInsertion```
+ ```OpenROAD.CheckAntennas```
+ ```OpenROAD.STAMidPNR-3```
+ ```OpenROAD.DetailedRouting```
+ ```Odb.RemoveRoutingObstructions```
+ ```OpenROAD.CheckAntennas-1```
+ ```Checker.TrDRC```
+ ```Odb.ReportDisconnectedPins```
+ ```Checker.DisconnectedPins```
+ ```Odb.ReportWireLength```
+ ```Checker.WireLength```
+ ```OpenROAD.FillInsertion```
+ ```Odb.CellFrequencyTables```
+ ```OpenROAD.RCX```
+ ```OpenROAD.STAPostPNR```
+ ```OpenROAD.IRDropReport```
## Signoff
+ ```Magic.StreamOut```
+ ```KLayout.StreamOut```
+ ```KLayout.Render```
+ ```Magic.WriteLEF```
+ ```Odb.CheckDesignAntennaProperties```
+ ```KLayout.XOR```
+ ```Checker.XOR```
+ ```Magic.DRC```
+ ```KLayout.DRC```
+ ```Checker.MagicDRC```
+ ```Checker.KLayoutDRC```
+ ```Magic.SpiceExtraction```
+ ```Checker.IllegalOverlap```
+ ```Netgen.LVS```
+ ```Checker.LVS```
## Timing Signoff
+ ```Checker.SetupViolations```
+ ```Checker.HoldViolations```
+ ```Checker.MaxSlewViolations```
+ ```Checker.MaxCapViolations```
+ ```Misc.ReportManufacturability```

## Ejecutando Linting
```shell
librelane --run-tag interactive_mode  counter.json --from Verilator.Lint -to Checker.LintWarnings 
```
Analizando la carpeta generada, nos podremos encontrar con los pasos que se ejecutaron, analizando cada subdirectorio dentro de *interactive_mode*, observamos que hay archivos llamados **state_in..json** y **state_out.json**

Estos archivos funcionan como un archivo para checar que se ejecuto de manera correcta el paso, y con ellos, podremos usarlos como referencia para ejecutar posteriores pasos siempre y cuando exista el archivo **state_out.json**

## Ejecutando la sintesis logica y fisica

```shell
librelane couter.json --last-run --with-initial-state runs/interactive_mode/04-checker-lintwarnings/state_out.json --from Yosys.JsonHeader --to Checker.NetlistAssignStatements
```

Explora las carpetas que se crearon

## Ejecutando floorplaning y placement

Para ejecutar esta etapa, usemos el siguiente comando:


```shell
librelane couter.json --last-run --with-initial-state runs/interactive_mode/09-checker-netlistassignments/state_out.json --from OPENROAD.CheckSDCFiles --to OpenROAD.DetailedPlacement
```

Explora las bases de datos, y abrelas con openroad

Para visualizar como abrirlas, usa lo siguiente: 

```shell 
openroad -gui
```

Dentro de la gui, usa las siguientes variables acorde a tu diseno
```tcl
read_lib $env(PDK_ROOT)/sky130A/libs.ref/sky130_fd_sc_hd/lib/sky130_fd_sc_hd__ss_100C_1v60.lib
read_db odb/counter.odb 
read_spef spef/max/counter.max.spef
read_sdc sdc/counter.sdc
```

## Ejecutando CTS

Para ejecutar CTS, usemos los siguientes pasos:

```shell
librelane couter.json --last-run --with-initial-state runs/interactive_mode/35-openroad-cts/state_out.json --from OpenROAD.CTS --to OpenROAD.STAMidPNR
```

Para abrir la db usamos:

```shell
librelane counter.json --last-run --flow OpenInOpenROAD 
```

## Ejecutando Routing

Para ejecutar la etapa de Routing, usemos los siguientes pasos:

```shell
librelane couter.json --last-run --with-initial-state runs/interactive_mode/71-checker-lvs/state_out.json --from Checker.SetupViolations --to OpenROAD.IRDropReport
```

## Signoff 

Para ejecutar la etapa de Routing, usemos los siguientes pasos:

```shell 
librelane couter.json --last-run --with-initial-state runs/interactive_mode/72-checker-setupviolations/state_out.json --from OpenROAD.GlobalRouting --to Misc.ReportManufacturability
```

