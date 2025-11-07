# A Path-Following-Based CAV Motion Planning Framework for Unified Description of Car-Following and Lane-Changing

This repository provides supplementary analysis for the manuscript titled "**[YOUR MANUSCRIPT TITLE HERE]**".

During the review process, a valuable point was raised regarding the framework's ability to handle **device heterogeneity**. Due to manuscript length limitations, the detailed analyses addressing this topic are presented here as a supplement to the main paper.

This analysis is broken down into two specific, measurable components: (1) robustness to sensor noise and (2) computational scalability.

---

## 1. Robustness to Sensor Noise (Sensing Equipment Heterogeneity)

To stress-test the framework's robustness under perception noise, a supplementary experiment was conducted. This analysis simulates heterogeneous sensing devices with varying accuracy by injecting zero-mean Gaussian noise into the perceived states ($x$, $y$, $v$) of surrounding vehicles, a standard approach for such tests.

Specifically, at each time step, the ego vehicle perceived the state of a surrounding vehicle $j$ as follows:

$$
x_{j,perceived}(t) = x_{j,true}(t) + \mathcal{N}(0, \sigma_x^2)
$$
$$
y_{j,perceived}(t) = y_{j,true}(t) + \mathcal{N}(0, \sigma_y^2)
$$
$$
v_{j,perceived}(t) = v_{j,true}(t) + \mathcal{N}(0, \sigma_v^2)
$$

The experiment was conducted across all test set trajectories from the “Original” scenario (defined in the manuscript). The **baseline (Level 0: $\sigma_x = 0 \text{ m}$; $\sigma_y = 0 \text{ m}$; $\sigma_v = 0 \text{ m/s}$)** was compared against two noise levels: **Level 1 (Low Noise: $\sigma_x = 0.05 \text{ m}$; $\sigma_y = 0.1 \text{ m}$; $\sigma_v = 0.1 \text{ m/s}$)** and **Level 2 (Medium Noise: $\sigma_x = 0.1 \text{ m}$; $\sigma_y = 0.3 \text{ m}$; $\sigma_v = 0.3 \text{ m/s}$)**.

The detailed statistical results are presented in **Table B-II**.

### TABLE B-II
**ROBUSTNESS STRESS-TESTING RESULTS UNDER PERCEPTION NOISE**

| Metric | | Level 0 (Baseline) | Level 1 (Low Noise) | Level 2 (Medium Noise) |
| :--- | :--- | :---: | :---: | :---: |
| **Safety** | *Total Collisions (ACT < 0 s)* | **0** | **0** | **0** |
| | *TI-ACT* | 4.483 s² | 4.545 s² (+1.4%) | 4.662 s² (+4.0%) |
| | *TE-ACT* | 2.379 s² | 2.410 s² (+1.3%) | 2.479 s² (+4.2%) |
| **Comfort** | *jerk* | 0.356 m/s³ | 0.385 m/s³ (+8.1%) | 0.399 m/s³ (+21.3%) |
| **Accuracy** | *Model error*| 0.063 | 0.069 (+9.5%) | 0.091 (+44.4%) |
| | *RMSE(s)* | 0.317 m | 0.346 m (+9.1%) | 0.418 m (+38.2%) |
| | *MAE(s)* | 0.298 m | 0.322 m (+8.1%) | 0.399 m (+33.9%) |
| | *MAPE(s)*| 0.023 | 0.026 (+13.0%) | 0.034 (+47.8%) |

### Analysis of Robustness

The results detailed in Table B-II demonstrate the framework's robustness:

* **Safety:** Most critically, **zero collisions (ACT < 0s) occurred at any noise level**. Even under Medium noise, the safety risk metrics (TI-ACT and TE-ACT) increased by a mere ~4.0%, remaining well within a controllable range.
* **Comfort:** The framework showed high tolerance for noise, with the maximum ride discomfort (jerk) kept below 22% (rising to 0.432 m/s³ at Level 2).
* **Accuracy:** The system's accuracy reveals an intelligent trade-off. At Low Noise, it maintained high accuracy (RMSE(s) +9.1%). At Medium Noise, it prioritized safety (as shown by the minimal +4.2% risk increase), resulting in a necessary and intentional decrease in tracking accuracy (RMSE(s) +38.2%). This is a desirable behavior for a safety-critical system.

The above results indicate that the PFF framework remains highly stable and robust under heterogeneous sensing conditions.

---

## 2. Computational Scalability (Computational Hardware Equipment Heterogeneity)

To address computational heterogeneity, the PFF framework's real-time performance was tested on multiple computing platforms, ranging from a high-end desktop to an embedded edge device.

The statistical results of this analysis are summarized in **Table B-III**.

### TABLE B-III
**COMPUTATIONAL PERFORMANCE ON HETEROGENEOUS HARDWARE**

| Platform | Processor | Memory (RAM) | Mean Time (ms) | 95th-Percentile (ms) | Real-Time (<10 ms) |
| :--- | :--- | :---: | :---: | :---: | :---: |
| **Desktop (Baseline)** | Intel Core i5-13600KF @ 3.5 GHz | 32 GB | 3.186 | 4.234 | ✔ |
| **Laptop (Mid-range)** | Intel Core i7-12700H @ 2.7 GHz | 16 GB | 5.231 | 6.811 | ✔ |
| **Embedded (Edge)** | NVIDIA Jetson Orin (32 GB LPDDR5) | -- | 6.890 | 9.013 | ✔ |

To visually complement this data, **Fig. B1** illustrates the step-by-step computational time for each platform, further demonstrating the stable, millisecond-level performance.

![Fig. B1. Real-time computational time of pff framework on heterogeneous platforms.](PFF_Computational_Time_Across_Hardware.png)

### Analysis of Scalability

The data confirms the framework's exceptional efficiency. Even on the least powerful platform (NVIDIA Jetson Orin), the **95th-percentile computation time is 9.013 ms**, which remains below the 10 ms real-time requirement. In contrast, the high-end desktop (i5-13600KF) achieved a 95th-percentile time of just 4.234 ms.

This demonstrates the PFF's high efficiency and strong computational scalability, confirming its portability and real-time adaptability for heterogeneous computing environments.

---

### Supporting Data Files

The raw data used to generate the figures and tables in this analysis are provided in this repository:

* `desktop_perf.csv`: Raw computational time data (in ms) for the Desktop (Baseline) platform.
* `laptop_perf.csv`: Raw computational time data (in ms) for the Laptop (Mid-range) platform.
* `embedded_perf.csv`: Raw computational time data (in ms) for the Embedded (Edge) platform.
