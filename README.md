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

<p align="center">
<img width="363" height="517" alt="image" src="https://github.com/user-attachments/assets/70f949b0-381f-4940-856b-10b651f5a128" />
</p>

_Reference: Plexim, "Space Vector Control of a Three-Phase Rectifier using PLECS," Available: https://www.plexim.com/sites/default/files/plecs_svm.pdf_

___
 
* **Step 1: Three-Phase to α-β Transformation**

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

<p align="center">
<img width="1376" height="392" alt="image" src="https://github.com/user-attachments/assets/b8ad6e13-9a6a-4708-b9c2-fa35c631cebd" />
</p>
___

* **Sector Identification**

The reference vector rotates along a circle with a hexagon inscribed in it. This hexagon is divided into six sectors of 60&deg; each. By identifying the sector, we can use the respective Vα and Vβ space vectors to calculate the reference vector.

The angle calculated in the above steps varies from -pi radians to +pi radians hence; it is converted to a range og 0 radians to 2pi radians. 
A simple check is done to check whether the angle is less than 0 radians. If it is TRUE then 2pi is added to the angle else the angle is passed as it is.
After this the angle is converted from radians to degrees. 

**NOTE: The angle conversion to degrees is not a necessary step. The sector identification can be done with the angle in radians too.**

The angle is then used to do a comparison with some constants checking whether the angle is in between the limits of each sector such that;

0&deg; < angle < 60&deg; -> Sector 1

60&deg; < angle < 120&deg; -> Sector 2

120&deg; < angle < 180&deg; -> Sector 3

1800&deg; < angle < 240&deg; -> Sector 4

240&deg; < angle < 300&deg; -> Sector 5

300&deg; < angle < 360&deg; -> Sector 6

At the end, we will get the sector number between from 1 to 6 depending on angle of the reference vector.

<p align="center">
<img width="1742" height="725" alt="image" src="https://github.com/user-attachments/assets/b7616c88-76a0-4a51-86f2-88164ebd1da5" />
</p>

* **Time Calculation for Active Vectors**

A two-level inverter has 8 switching states, producing:

6 active vectors; V1 to V6 and

2 zero vectors V0, V7

After identifying the sector in the above step, we can decide which of the active vectors we can use. For each active vector, we need to determine for how that active vector should be applied. It is also called Dwell Time. In this step, we will be calculating the time for these active vectors. 

The equations for calculating the dwell times is given below;

