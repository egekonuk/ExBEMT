# ExBEMT: An Extended Blade Element Momentum Theory Solver

![image](https://github.com/user-attachments/assets/3ab02815-5df5-4a92-b018-612111ecc945)

[![MATLAB](https://img.shields.io/badge/MATLAB-R2024a%2B-blue)](https://www.mathworks.com)

**ExBEMT (aka Extended-BEMT) is a modular BEMT-Based tool with some improvements for small-scale propellers and VTOL proprotors, optimized for high-incidence conditions and real-time simulation. Fast, accurate, and built for early-stage design and testing.**

---

## Introduction & Vision

**Version 1.0 (06/25/2025)**

Welcome to ExBEMT! My name is Ege Konuk, and this tool is a direct result of my PhD research in Aerospace Engineering at Old Dominion University. My work specializes in propeller performance prediction and developing reduced-order aerodynamic models for electric aircraft.

I believe in the power of simplicity and in making complex aerodynamic problems digestible for practical use. With ExBEMT, my goal was to embody this approach by creating a tool with a very gentle learning curve. I wanted it to be powerful enough for detailed research yet intuitive enough to become a go-to utility in any engineer's or enthusiast's toolkit. I hope it aids you in your own design projects, learning, and discovery :)

**Version 2.0 (07/21/2025)**

This update focuses on significantly improving the flexibility, robustness, and usability of the ExBEMT solver.

**Major Features & Enhancements**

**1. Dynamic Propeller Geometry Loading**

The method for loading propeller data from Prop_sections.xlsx has been completely overhauled.

Automatic Section Detection: The GUI no longer relies on a fixed Excel range (e.g., A1:R8). It now dynamically detects the number of blade sections by reading the data in the sheet. This allows users to define propellers with any number of sections without needing to modify the code.

Robust Data Reading: The code now uses readcell to better handle the mixed data types (numbers and text for foil names) in the Excel file, making the data import process more reliable.

**2. Improved XFOIL Analysis Workflow**

The interaction with XFOIL has been refined for better performance and stability.

Streamlined XFOIL Call: The xfoil_call function has been updated to be more robust, managing file paths and system calls more effectively.

Organized File Structure: XFOIL input/output files are now managed in dedicated directories to prevent conflicts and keep the project folder clean.

**3. Enhanced User Interface & Experience**

Improved Logging: The log now provides more detailed feedback during the XFOIL analysis, informing the user which section and airfoil is currently being processed.

**Bug Fixes & Code Refinements**

General Stability: Refactored various parts of the code to improve error handling and provide clearer error messages to the user.

**Future Development**

Implement Viterna Post-Stall Model: Integrate the Viterna method to extrapolate airfoil data to high angles of attack (±180 
∘
 ). This will provide a more accurate and physically realistic drag polar for stalled conditions, improving overall solver fidelity.

Parallel XFOIL Runs: Implement parfor loops to run XFOIL analyses for multiple blade sections in parallel, significantly speeding up the polar generation process.

Advanced Post-Processing: Add more tabs for advanced plots, such as blade loading distributions (dT/dr, dQ/dr) and inflow angle distributions.

GUI for Airfoil Management: Create a tool within the GUI to manage, plot, and filter airfoil .dat files.

---

## Key Features

* **Interactive GUI:** A user-friendly graphical interface to easily set up and run complex analyses without modifying code.
* **Multiple Fidelity Models:** Seamlessly switch between different analysis models:
    * Standard Blade Element Momentum Theory (BEMT)
    * BEMT with a steady-state Pitt-Peters Dynamic Inflow model
    * BEMT with a Quasi-Unsteady Pitt-Peters Dynamic Inflow model
* **Automated Airfoil Analysis:** Integrates XFOIL to automatically generate or load airfoil polar data for different propeller sections.
* **Comprehensive Plotting:** Automatically generates performance curves (Thrust, Torque, Normal Force, Efficiency) and detailed azimuthal contour plots for in-depth analysis of blade loading.
* **Flexible Analysis Modes:** Analyze propeller performance over a range of advance ratios, velocities, or angles of incidence.
* **Modular Propeller Data:** Easily add and manage different propeller geometries through a simple folder structure.

---

## Showcase

### Main Application Interface

The intuitive GUI allows for complete control over the simulation parameters, from atmospheric conditions and RPM to the specific analysis range and fidelity model.

![ExBEMT-Window](https://github.com/user-attachments/assets/3341133c-198a-443a-907d-3391ddf8106c)

![ExBEMT_analysis_AOI](https://github.com/user-attachments/assets/5998a493-6020-41fc-b362-c273f3714392)

### Detailed Azimuthal Contour Plots

Visualize the aerodynamic loads and flow conditions across the entire propeller disk for any flight condition. The tool automatically generates comparison plots to show the effects of different inflow models.

![ExBEMT_Contours](https://github.com/user-attachments/assets/0156d7ad-16bb-47e4-99ee-b95519d3c28e)

---

## Analysis Models

ExBEMT implements several models to provide a range of fidelity and computational speed:

1.  **Baseline BEMT:** A robust and fast implementation of Blade Element Momentum Theory, suitable for initial performance estimates.
2.  **BEMT + Pitt-Peters Dynamic Inflow (Steady):** Incorporates the classic Pitt-Peters dynamic inflow model to capture the effects of non-uniform induced velocity, providing more accurate results for off-axis flow and transient conditions.
3.  **BEMT + Unsteady Pitt-Peters Dynamic Inflow:** The highest fidelity model, which implements the unsteady formulation of the Pitt-Peters model to capture time-varying induced velocity effects, making it ideal for high-incidence and complex VTOL flight regimes.

---

## Getting Started

### Prerequisites

* MATLAB (R2024a or newer)
* MATLAB Aerospace Toolbox (for `atmosisa` and `convangvel` functions)
* XFOIL: The executable (`xfoil.exe` for Windows) must be included in the project's root directory or in your system's PATH.

### Installation & Running

1.  **Clone the Repository:**
    ```bash
    git clone [https://github.com/egekonuk/ExBEMT.git](https://github.com/egekonuk/ExBEMT.git)
    ```
2.  **Open in MATLAB:** Navigate to the cloned repository folder in MATLAB.
3.  **Run the App:** Open the `ExBEMT_GUI.m` file in the editor and click the **Run** button, or type the following in the MATLAB command window:
    ```matlab
    ExBEMT_GUI
    ```

---

## How to Use

1.  **Select a Propeller:** Choose a propeller from the "Propeller" dropdown menu. (See "Adding New Propellers" below).
2.  **Set General Settings:** Input the flight altitude, number of blades, and RPM.
3.  **Choose Analysis Type:** Select whether to analyze over a range of Advance Ratio, Velocity, or Angle of Incidence.
4.  **Define Analysis Range:** Set the min, max, and step values for your chosen analysis type.
5.  **Select Fidelity Model:** Under "Corrections & Models", choose the desired analysis model.
6.  **Run Analysis:** Click the **"Run Analysis"** button to begin the simulation.
7.  **View Results:** Performance plots will appear in the "Output" panel. If "Plot Contour Plots" is checked, new figure windows will be generated for each iteration.

---

## Adding New Propellers

ExBEMT is designed to be modular. To add your own propeller, create a new folder inside the `Propeller Packages` directory. This folder must contain:

1.  **`Prop_sections.xlsx`:** An Excel file detailing the blade geometry. It should follow the format of the existing examples.
2.  **Airfoil `.dat` files:** Coordinate files for each airfoil used in the propeller design, with names corresponding to those in the `Prop_sections.xlsx` file.

Once the folder is created, restart the GUI, and the new propeller will appear in the dropdown menu.

---

## Acknowledgements

The automated, parallel-processing interface with XFOIL was made possible thanks to the excellent `XFOILinterface` class developed by **Rafael Fernandes de Oliveira**. This tool is highly recommended for anyone looking to integrate XFOIL with MATLAB.

* [theolivenbaum/XFOILinterface on GitHub](https://github.com/theolivenbaum/XFOILinterface)

---

## License

This project is licensed under the GNU General Public License v3.0 - see the [LICENSE](https://github.com/egekonuk/ExBEMT--An-Extended-Blade-Element-Momentum-Theory-Solver/blob/main/LICENSE) file for details.

---

## Citation

If you use ExBEMT in your research, please consider citing it:


Konuk, E. (2025).GitHub. https://github.com/egekonuk/ExBEMT


Or, by citing my PhD Dissertation:

Konuk E., "Computer Based Modeling for Small e-VTOL Propeller Performance," Ph.D Dissertation, Old Dominion University, 2024. https://digitalcommons.odu.edu/mae_etds/627/ DOI: 10.25777/8agh-jw47


## Contact

For questions, issues, or collaboration, please open an issue on GitHub or contact me :)




