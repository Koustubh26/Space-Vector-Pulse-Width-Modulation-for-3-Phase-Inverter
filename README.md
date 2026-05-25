# Space-Vector-Pulse-Width-Modulation-for-3-Phase-Inverter

📘**Introduction**

This projects presents a simulation of Two-Level Space Vector Pulse Width Modulation (SVPWM), a widely used modulation technique in power electronics for controlling voltage source inverters. A comparative study is performed on the two different types of modulation strategies using SVPWM; 

- Symmetrical Modulation
- Alternating Zero Vector Modulation

SVPWM is very popular for controlling inverters because of its higher efficiency offering 15% increase in the DC link voltage utilization and low output harmonic distortions in the output waveform compared to the conventional sinusoidal PWM. Its output is similar to the output of a Third Harmonic injected inverter in terms of voltage output.

The project focuses on implementing and analyzing a two-level inverter topology using SVPWM principles. Simulations are performed in Simulink and basic mathematical equations are used to design the model instead of using the readily available blocks from MATLAB for a better and basic understanding of the modulation strategies.

Key objectives of this repository include:

- Understanding the fundamentals of SVPWM for two-level inverters
- Developing a simulation model for generating switching signals
- Visualizing space vectors, sector identification, and switching sequences
- Comparative study of the two modulation strategies in terms of waveform quality and harmonic content

This work is particularly useful for students, researchers, and engineers interested in power electronics, motor drives, and inverter control techniques who would like to understand the basics of the technique, how it is implemented and the shape of the waveforms at different points.

___

🧠 **Theory**

SVPWM treats the three-phase inverter output as a single rotating vector in a two-dimensional α-β plane (also known as Stationary Reference frame or Clarke transformation plane). In this approach, the three sinusoidal phase quantities Va, Vb,and Vc are transformed into two orthogonal components Vα and Vβ, which together form a complex space vector Vref. 

Vref = Vα + jVβ --------------------------------------------------(1)

This reference vector Vref represents the instantaneous magnitude and direction of the desired three-phase voltage system. As time progresses, Vref rotates in the α-β plane at the fundamental frequency, effectively capturing the behavior of the three-phase signals in a geometrically intuitive form. This eliminates the need to control each phase independently and instead allows unified control using vector synthesis.

___

🧮 **Step by Step Explanation**

The below image explains the block diagram of the technique;

<img width="363" height="517" alt="image" src="https://github.com/user-attachments/assets/70f949b0-381f-4940-856b-10b651f5a128" />

_Reference: Plexim, "Space Vector Control of a Three-Phase Rectifier using PLECS," Available: https://www.plexim.com/sites/default/files/plecs_svm.pdf_

___
 
**Step 1: Three-Phase to α-β Transformation**

The three-phase voltages Va, Vb, and Vc are first transformed into a two-axis stationary reference frame (α-β plane) using the Clarke Transformation.

The three-phase voltages are given by;
<p align="center">
Va = VmCos(ωt)
<p align="center">
Vb = VmCos(ωt-120&deg;)
<p align="center">
Vc = VmCos(ωt-240&deg;)
</p>

The two vectors Vα and Vβ are calculated as shown below;

<p align="center">
<img src="https://latex.codecogs.com/svg.image?\begin{bmatrix}V_\alpha\\V_\beta\end{bmatrix}=\frac{2}{3}\begin{bmatrix}1&-\frac{1}{2}&-\frac{1}{2}\\0&\frac{\sqrt{3}}{2}&-\frac{\sqrt{3}}{2}\end{bmatrix}\begin{bmatrix}V_a\\V_b\\V_c\end{bmatrix}" />
</p>

The Reference vector is given by;

$$
\vec{V}_{ref} = V_{\alpha} + j V_{\beta}
$$

