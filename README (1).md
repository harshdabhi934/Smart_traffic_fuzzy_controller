# Design and Implementation of a Fuzzy Logic Controller for Traffic Signal Timing

## College Project

A fuzzy-logic-based traffic signal controller that recommends the required **green-light duration** using two traffic conditions:

- **Queue Length** — number of vehicles waiting
- **Waiting Time** — average/observed waiting time

The project implements a **Mamdani-type Fuzzy Inference System (FIS)** using Python and `scikit-fuzzy`. It covers the fuzzy design, testing, controller analysis, defuzzification comparison, RMSE analysis, and an interactive Jupyter Notebook interface.

> **Course:** Fuzzy Logic & Genetic Programming  
> **Academic Year:** 2026–27  
> **Branch:** Civil Engineering  
> **Semester:** V  
> **Institution:** L.D. College of Engineering, Ahmedabad

---

## 1. Project Overview

Conventional fixed-time traffic signals use predetermined green-light durations. Such systems may not respond effectively to changing traffic demand.

This project proposes a fuzzy logic controller that uses:

1. **Queue Length:** 0–50 vehicles
2. **Waiting Time:** 0–60 seconds

to determine:

3. **Recommended Green Time:** 0–60 seconds

The controller uses fuzzy membership functions and a nine-rule Mamdani rule base to convert traffic conditions into a recommended green duration.

### Main objective

To develop and evaluate a fuzzy traffic signal controller that can provide gradual and flexible green-time decisions instead of relying only on fixed thresholds.

---

## 2. Project Features

- Mamdani fuzzy inference system
- Two fuzzy input variables
- One fuzzy output variable
- Triangular membership functions for the main controller
- Alternative trapezoidal and Gaussian membership-function designs
- Nine fuzzy rules
- Centroid defuzzification as the primary method
- Comparison of five defuzzification methods:
  - Centroid
  - Bisector
  - Mean of Maximum (MOM)
  - Smallest of Maximum (SOM)
  - Largest of Maximum (LOM)
- Testing with 12 traffic conditions
- Controller behaviour analysis
- Monotonicity checks
- Fuzzy output surface visualization
- MSE and RMSE comparison
- Interactive Jupyter Notebook interface using `ipywidgets`

---

## 3. System Architecture

```text
             Traffic Conditions
                    |
          +---------+---------+
          |                   |
   Queue Length          Waiting Time
   0–50 vehicles         0–60 seconds
          |                   |
          +---------+---------+
                    |
             Fuzzification
                    |
                    v
          +-------------------+
          |   Fuzzy Rule Base |
          |    9 Mamdani Rules|
          +-------------------+
                    |
                    v
             Fuzzy Inference
                    |
                    v
             Defuzzification
                    |
                    v
          Recommended Green Time
               0–60 seconds
```

---

## 4. Fuzzy Variables

### 4.1 Queue Length

Universe of discourse:

**0–50 vehicles**

Membership functions:

| Fuzzy Set | Type | Parameters |
|---|---|---|
| Low | Triangular | [0, 10, 25] |
| Medium | Triangular | [10, 25, 40] |
| High | Triangular | [25, 40, 50] |

### 4.2 Waiting Time

Universe of discourse:

**0–60 seconds**

Membership functions:

| Fuzzy Set | Type | Parameters |
|---|---|---|
| Short | Triangular | [0, 15, 30] |
| Medium | Triangular | [15, 30, 45] |
| Long | Triangular | [30, 45, 60] |

### 4.3 Green Time

Universe of discourse:

**0–60 seconds**

Membership functions:

| Fuzzy Set | Type | Parameters |
|---|---|---|
| Short | Triangular | [10, 20, 30] |
| Medium | Triangular | [20, 35, 50] |
| Long | Triangular | [40, 50, 60] |

---

## 5. Fuzzy Rule Base

The controller uses nine rules covering the combinations of queue length and waiting time.

| Rule | Queue Length | Waiting Time | Green Time |
|---|---|---|---|
| R1 | Low | Short | Short |
| R2 | Low | Medium | Short |
| R3 | Low | Long | Medium |
| R4 | Medium | Short | Medium |
| R5 | Medium | Medium | Medium |
| R6 | Medium | Long | Long |
| R7 | High | Short | Medium |
| R8 | High | Medium | Long |
| R9 | High | Long | Long |

The rule base is designed so that higher traffic demand and longer waiting times generally receive a longer green duration.

---

## 6. Fuzzy Inference Process

The controller follows the standard fuzzy-control process:

### Step 1 — Fuzzification

Crisp traffic values are converted into degrees of membership in fuzzy sets.

Example:

```text
Queue Length = 18 vehicles
Waiting Time = 22 seconds
```

These values are evaluated against the corresponding fuzzy membership functions.

### Step 2 — Rule Evaluation

The nine fuzzy rules are evaluated using the membership values of queue length and waiting time.

### Step 3 — Aggregation

The outputs of all activated rules are combined to produce a fuzzy green-time output.

### Step 4 — Defuzzification

The fuzzy output is converted into a crisp green-time value.

The project uses **Centroid** as the primary defuzzification method.

---

## 7. Defuzzification Methods

The notebook allows comparison of five methods:

### Centroid

Calculates the centre of gravity of the aggregated fuzzy output.

**Primary method used by this project.**

### Bisector

Divides the aggregated fuzzy area into two equal areas.

### MOM — Mean of Maximum

Calculates the average of all values having maximum membership.

### SOM — Smallest of Maximum

Selects the smallest value having maximum membership.

### LOM — Largest of Maximum

Selects the largest value having maximum membership.

---

## 8. Alternative Membership Functions

The project also creates alternative membership-function designs for comparison.

### Trapezoidal Membership Functions

Used as an alternative to triangular membership functions.

### Gaussian Membership Functions

Used to investigate a smoother membership-function shape.

These alternatives are maintained separately and are **not mixed into the primary triangular-membership controller**.

---

## 9. Phase 4 — Testing

The controller is tested using **12 different input combinations** covering light, moderate, heavy, severe, and extreme congestion conditions.

Example test cases include:

| Queue Length | Waiting Time | Condition |
|---:|---:|---|
| 2 | 3 | Light traffic |
| 5 | 15 | Light traffic |
| 10 | 20 | Moderate traffic |
| 18 | 22 | Moderate traffic |
| 20 | 35 | Moderate congestion |
| 30 | 45 | Heavy congestion |
| 40 | 50 | Severe congestion |
| 45 | 55 | Very severe congestion |
| 48 | 55 | Extreme congestion |

The notebook generates:

- Test result table
- Green-time output graph
- Light-traffic demonstration
- Congested-traffic demonstration

---

## 10. Phase 5 — Controller Analysis

The analysis evaluates how the controller responds to changes in traffic conditions.

### Expected behaviour

- Increasing queue length should generally increase green duration.
- Increasing waiting time should generally increase priority.
- The fuzzy controller provides gradual changes rather than one hard threshold.
- The output remains within the defined green-time universe.

### Monotonicity Analysis

The notebook checks whether green time is non-decreasing when:

1. Queue length increases while waiting time is fixed.
2. Waiting time increases while queue length is fixed.

Several fixed waiting-time and fixed queue-length values are tested.

---

## 11. Output Surface

A two-dimensional fuzzy output surface is generated using:

- X-axis: Queue Length
- Y-axis: Waiting Time
- Z/value: Recommended Green Time

This visualization helps demonstrate how the controller changes green duration across different combinations of traffic demand and waiting conditions.

---

## 12. RMSE Analysis

The project compares the five defuzzification methods using MSE and RMSE.

### Important interpretation

The notebook does **not** contain measured real-world green-time target values.

Therefore, the RMSE calculation is a **relative comparison against the project's Centroid output**, not a measure of real-world prediction accuracy.

The comparison is useful for studying how much the alternative defuzzification methods differ from the selected Centroid method.

---

## 13. Phase 6 — Interactive User Interface

The notebook includes an interactive interface using `ipywidgets`.

### User inputs

- Queue Length: 1–50 vehicles
- Waiting Time: 1–60 seconds
- Defuzzification method

### Available methods

```text
Centroid
Bisector
MOM
SOM
LOM
```

### Output

The interface displays:

- Queue Length
- Waiting Time
- Traffic Status
- Selected Defuzzification Method
- Recommended Green Time
- Fuzzy output visualization

### Traffic classification used by the interface

| Condition | Classification |
|---|---|
| Queue ≤ 10 and Waiting ≤ 20 | Light traffic |
| Queue ≤ 25 and Waiting ≤ 35 | Moderate traffic |
| Queue ≤ 40 and Waiting ≤ 50 | Heavy congestion |
| Other conditions | Severe congestion |

---

## 14. Technologies Used

### Programming Language

- Python

### Libraries

- NumPy
- Pandas
- Matplotlib
- SciPy
- scikit-fuzzy
- ipywidgets

### Development Environment

- Jupyter Notebook / JupyterLab

---

## 15. Installation

Install the required Python packages using:

```bash
pip install -U scikit-fuzzy scipy matplotlib numpy pandas ipywidgets
```

If using Jupyter Notebook or JupyterLab, make sure the notebook kernel is using the same Python environment where these packages are installed.

---

## 16. How to Run the Project

### Step 1 — Open the notebook

Open:

```text
smart_traffic_fuzzy_controller_FINAL_PHASE3_4_5_6(2).ipynb
```

in Jupyter Notebook or JupyterLab.

### Step 2 — Install dependencies

Run the installation cell if the required libraries are not already installed.

### Step 3 — Run the notebook

Run the cells from top to bottom.

### Step 4 — View fuzzy membership functions

The notebook displays the membership functions for:

- Queue Length
- Waiting Time
- Green Time

### Step 5 — Run testing

Execute the Phase 4 cells to generate the 12 test cases and output graph.

### Step 6 — Run analysis

Execute the Phase 5 cells to generate:

- Controller response analysis
- Monotonicity checks
- Output surface
- Defuzzification comparison
- RMSE/MSE comparison

### Step 7 — Run the interface

Execute the Phase 6 cells to open the interactive traffic signal controller.

---

## 17. Example Workflow

Suppose:

```text
Queue Length = 40 vehicles
Waiting Time = 50 seconds
```

The controller:

```text
Input Traffic Data
        ↓
Fuzzification
        ↓
Nine Fuzzy Rules
        ↓
Mamdani Inference
        ↓
Centroid Defuzzification
        ↓
Recommended Green Time
```

The exact numerical output should be obtained by running the notebook rather than assuming a fixed value.

---

## 18. Project Limitations

The current project has several limitations:

1. Membership-function boundaries are manually selected.
2. The rule base is based on design assumptions rather than calibrated field data.
3. No real-time traffic sensor stream is connected.
4. RMSE is calculated relative to the Centroid method rather than measured real-world green times.
5. The model represents one signal decision and does not simulate a complete multi-intersection traffic network.

---

## 19. Future Scope

The system can be extended by:

- Calibrating membership functions using historical traffic data.
- Connecting real-time vehicle-count sensors.
- Using real-time waiting-time measurements.
- Adding lane occupancy as an input.
- Adding vehicle arrival rate.
- Adding pedestrian demand.
- Adding emergency-vehicle priority.
- Comparing against conventional fixed-time signal control.
- Measuring actual queue reduction and vehicle delay.
- Using SUMO for traffic-network simulation.
- Extending the controller to multiple intersections.

---

## 20. Project Phases

| Phase | Work Completed |
|---|---|
| Phase 3 | Fuzzy system design, membership functions, rule base and defuzzification |
| Phase 4 | Testing with 12 input combinations, tables, graphs and demonstrations |
| Phase 5 | Behaviour analysis, monotonicity, output surface, defuzzification comparison and RMSE |
| Phase 6 | Interactive Jupyter-based user interface |

---

## 21. Project Structure

```text
Project/
│
├── smart_traffic_fuzzy_controller_FINAL_PHASE3_4_5_6(2).ipynb
└── README.md
```

Additional screenshots, reports, presentations, or datasets can be added as required for college submission.

---

## 22. Authors

| Name | LinkedIn |
|---|---|
| Your Name | [linkedin.com/in/demo-user](https://www.linkedin.com/in/demo-user) |

> **Note:** Replace `demo-user` with your actual LinkedIn username/profile URL.

---

## 23. Academic Note

This project is developed as a **college academic project** to demonstrate the application of fuzzy logic to traffic signal timing.

The controller is a simulation/prototype and should not be treated as a production traffic-signal control system without validation using real traffic data, safety requirements, traffic standards, and field testing.

---

## 23. Conclusion

The project demonstrates the design and implementation of a Mamdani fuzzy logic controller for adaptive traffic signal timing.

By using **queue length** and **waiting time** as inputs, the system produces a recommended **green-light duration** through a transparent nine-rule fuzzy system.

The notebook extends the initial fuzzy design through:

- System testing
- Controller behaviour analysis
- Monotonicity checks
- Output-surface visualization
- Defuzzification comparison
- Relative RMSE analysis
- Interactive user interface

The **Centroid defuzzification method** is retained as the primary method, while Bisector, MOM, SOM, and LOM are included for comparison.

---

**College Project — Fuzzy Logic & Genetic Programming**

**Branch:** Civil Engineering  
**Semester:** V  
**Academic Year:** 2026–27  
**Institution:** L.D. College of Engineering, Ahmedabad

---

## License

This project is intended for academic and educational use.
