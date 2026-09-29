# CO2-Storage-Capacity-Estimation-in-Lower-Eagle-Ford-Kerogen-Nanopores
An image-derived alternative to the standard DOE/NETL volumetric method for estimating CO2 storage capacity in organic-rich shale, built on the pore-scale simulation pipeline from my Lower Eagle Ford (LEF) nanopore flow project.

## Background
The standard DOE/NETL method for estimating CO2 storage capacity in organic-rich shale already accounts for two trapping mechanisms, free-phase CO2 in open pore space, and CO2 adsorbed onto kerogen surfaces, using a Langmuir isotherm scaled against TOC (total organic carbon) content. That adsorption term, however, comes from a bulk empirical correlation (lab isotherm measurements plotted against TOC percentage), not from the rock's actual imaged pore geometry.

This project estimates both trapping mechanisms directly from real, imaged pore-scale structure instead, using the same FIB-SEM-derived pore network from my analytical + ML project.

## What this project does
- Reuses the statistically-equivalent pore network and CO2 invasion simulation from the analytical + ML project
- Computes free phase CO2 storage from pore volumes that remain CO2 occupied after simulated invasion
- Computes adsorbed-phase CO2 storage using a Langmuir isotherm, applied to the kerogen volume actually present within the simulated network's own physical scale (not mismatched against a macroscopic core plug volume, a scale-consistency detail addressed directly in the code)
- Reports storage capacity as an intensity (tonnes CO2 per m^3 of rock), scalable to any real target volume, rather than a single value tied to one arbitrary domain size
- Runs 200 Monte Carlo realizations to report storage intensity as a P10/P50/P90 risk range, consistent with how the analytical + ML project reports permeability and recovery factor

## Real data used, not placeholders
 - Kerogen volume fraction: 13.2 vol.%, directly measured from an independent PerGeos reconstruction of the same sample replacing an earlier density-derived estimate
 - Reservoir conditions (125C, 3500 psi) and TOC (5.75 wt.%) from Cudjoe et al. 2021 (Fuel 289)
 - Reconstruction domain volume (8.02 x 5.48 x 2.1 um): from the same PerGeos characterization, used as the default target volume for reporting absolute storage mass

## Results
Run against the same real 20 slice FIB-SEM dataset, 200 Monte Carlo realizations:                                                                                       - CO2 storage intensity: 0.102 tonnes CO2/m^3 rock most likely (0.076 conservative to 0.142 optimistic). Wider than permeability's range, consistent with recovery factor's known sensitivity to pore arrangement, since both share the same breakthrough-based logic.
- Both free-phase (pore space) and adsorbed-phase (kerogen-surface) trapping mechanisms actively contribute, using the real, directly-measured 13.2% kerogen volume fraction rather than a density-derived estimate.

## Tech stack
Python (Numpy, OpenPNM, PoreSpy, CoolProp for CO2 thermodynamic properties at reservoir conditions).
