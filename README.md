# Project-3-Enhancements
ECE 2774 University of Pittsburgh
Maria Luiza Hermann

## Purpose and theoretical background
The Power System Simulator is a Python-based tool for modeling and analyzing power systems through a modular, object-oriented design. Users can construct networks using components like buses, transmission lines, transformers, generators, and loads, and simulate system behavior under normal and faulted conditions using per-unit calculations. Core features include automated Ybus/Zbus matrix formation, Newton-Raphson power flow, and symmetrical/asymmetrical fault analysis with full sequence modeling.

To improve interpretability and user engagement, the simulator now includes graphical visualizations of voltage profiles, under the power flow and fault currents using matplotlib and networkx. These plots provide intuitive insights into system performance, line loading, and fault propagation. Designed as both a learning and analysis tool, the simulator reinforces key power system concepts while preparing users for real-world roles in planning, operations, and protection engineering.

One of the main reasons for plotting voltage profiles during power flow analysis is to get a clear picture of how the system behaves as it approaches a steady-state solution. Voltage magnitudes at each bus need to stay within safe operating limits (usually around ±5% of the nominal voltage) to ensure the proper functioning of equipment and the overall reliability of the grid. By tracking how voltage changes at a specific bus over the course of iterative methods like Newton-Raphson, engineers can see whether the system is converging correctly and identify any buses that might be causing issues. This approach is commonly recommended in power system analysis literature, as it helps validate both the numerical solution and the system configuration (Grainger & Stevenson, 1994).

Plotting voltage profiles before and after fault studies is equally important. It allows engineers to observe how a fault—whether it's a three-phase short circuit or an unbalanced line-to-ground fault—impacts the system voltages, particularly at sensitive or critical buses. These voltage dips can indicate how severe the fault is and help in designing better protection schemes. By comparing pre-fault and post-fault voltages visually, one can evaluate the system's ability to withstand and recover from disturbances, a practice grounded in symmetrical component theory and fault analysis methods (Anderson, 1995). This kind of analysis supports decisions related to system reinforcement and is essential for improving system resilience.

## Inputs and outputs structure
Inputs
* Bus Configuration: add_bus(name, base_kv) — Define bus names and their voltage levels
* Transmission Lines: add_conductor(...) — Define conductor type and electrical parameters [add_bundle(...) — Define conductor bundling, add_geometry(...) — Define line geometry, add_tline(name, from_bus, to_bus, bundle, geometry, length) — Connect buses with defined lines]
* Transformers: add_transformer(name, bus1, bus2, MVA, V1, V2, type, z_ground) — Add transformer and grounding impedance
* Generators: add_generator(name, bus, MW, MVAR, Vset, isPV) — Define generator capacity and bus type
* Loads: add_load(name, bus, MW, MVAR) — Specify load power demand at each bus
* Tracked Buses: set_tracked_buses(["Bus3", "Bus5", "Bus7"]) — Select buses to monitor voltage profiles
* Power Flow Settings: tolerance, max_iterations — Convergence criteria for Newton-Raphson solver
* Fault Study Parameters: 
  - User input via CLI: fault type (1–4) and faulted bus name
  - Assumed values: Vprefault = 1.0 + 0j, Zf = 0 for bolted faults

Outputs

* Bus Type Display: Prints classification of each bus (Slack, PV, PQ) to console
* Power Flow Solution: Bus voltages (p.u.) and phase angles (rad) after convergence
* Power mismatch (ΔP, ΔQ) at each bus: Formatted Jacobian matrix with labeled partial derivatives
* Voltage Profile Logging: voltage_profiles = {"Bus3": [("Initial", 1.0), ("Iteration 1", 0.98), ...]}
* Voltage Profile Plot: Line graph (matplotlib) of voltage vs. iteration/fault stage for tracked buses
* Fault Study Output: Fault current magnitude and angle

## Instruction for running
1. For running this simulator and it's enhancements it's necessary the following library: numpy pandas matplotlib
2. To ensure function install all python files in this github.
3. Alter any values in the file main.py, such as buses, lines, transformers, generators, and loads.
4. The main file already sets up the voltage tracking but you may alter what Bus you are trying to analyse it's voltage.
5. You may run the power flow in your main using solution.power_flow() and the voltage will be tracked automatically, each iteration necessary will have its own voltage recorded.
6. You may run the one of the fault studies in your main using solution.fault_study() and the voltage will be tracked automatically, each fault will have its own before and after voltage recordes.
7. The final action in main.py is solution.plot_all_voltage_profiles() this will print a list of the voltages tracked and will also graph them.


## Instructions for testing and validation
1. Structural Validation
* Buses are defined with correct base voltages.
* Generators are added on appropriate bus types (Slack or PV).
* Loads are assigned to PQ buses with correct real and reactive powers.
* Transformers and transmission lines are connected between the right bus pairs.
* A printed system summary confirms the system topology.

2. Power Flow Validation
* Newton-Raphson converges within a specified tolerance (e.g., 1e-6).
* The mismatch per iteration is printed and visibly reduces.
* Final voltage magnitudes and angles are printed per bus.
* Iteration count is reasonable (typically < 10).
* Results can optionally be compared to hand calculations on small test cases.

3. Voltage Profile Tracking Validation
* Tracked buses are correctly set using set_tracked_buses().
* The voltage_profiles dictionary records the correct number of stages.
* Voltage is tracked: [Initially, After each iteration of power flow, 
Before and after fault conditions]
* Plots using plot_all_voltage_profiles() show labeled trends.

4. Fault Analysis Validation
* Simulate all fault types: 3-phase, LG, LL, and LLG.
* Calculate fault currents and compare with expected formulas.
* Validate voltage changes at all buses post-fault.

5. Cross-Validation Using PowerWorld
* To confirm simulator accuracy with PowerWorld:
  **Build the same power system in PowerWorld (same buses, lines, loads, etc.).

* Run the power flow analysis and record:

** Voltage magnitude and angle at each bus

* Use "Export Case Info" to save results to CSV or Excel.

* Compare PowerWorld bus voltages and angles to your simulator outputs:

** Acceptable margin: ±0.001 p.u., ±0.1°

* For fault studies:

* Use PowerWorld’s Fault Analysis tool

* Match fault current and post-fault voltages from both tools

6. Visualization and Tracking

* Voltage labels such as "Initial", "Iteration 1", "Before Fault", etc. appear in the plots.

* All tracked buses show clearly separated line plots.

* Legends and axis labels are present and correctly formatted.

7. Regression Testing (Advanced)

* Store simulation outputs from a known stable version.

* Re-run same test after changes.

* Compare:

** Voltage profiles

** Final voltages/angles

** Fault results

* Use assertions or CSV comparisons to detect deviations.

## References
Grainger, J. J., & Stevenson, W. D. (1994). Power system analysis. McGraw-Hill.
Anderson, P. M. (1995). Analysis of faulted power systems. IEEE Press.
