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


The two-level inverter has eight switching states in which six are non-zero states (1 to 6) and two zero states (0 and 7). These eight switching states can be converted into eight space vectors respectively.

In each configuration, the vector identification uses logic notation '1' to denote positive phase voltage level (Upper transistor on, lower transistor off) and logic symbol '0' to denote negative phase voltage level ( Upper transistor off, lower transistor on).

The relationship between the vector space and the corresponding switching states is shown in the figure below;

<p align="center">
<img width="602" height="432" alt="image" src="https://github.com/user-attachments/assets/d243b6e5-2607-4f52-99b2-e4635516e629" />
</p>

Six non-zero voltage vectors are represented on graph space forming hexagonal orbits, of equal magnitude, out of phase by 60 degrees. It can be seen that when the space vector moves from one corner to another of the hexagon, only the state of one inverter pin changes. Zero space vectors are at the origin of the coordinate system.

The space vector diagram is shown below;

<p align="center">
<img width="472" height="417" alt="image" src="https://github.com/user-attachments/assets/34ca0b16-860b-42ef-8c30-7a703995f60a" />
</p>

To generate a sinusoidal three-phase voltage we have to construct a vector that rotates in plane space called the reference voltage vector $\vec{V}_{ref}$. The reference voltage vector rotates in space with angular velocity $\omega = 2\pi f$, where f is the fundamental frequency of the output voltage effectively capturing the behavior of the three-phase signals in a geometrically intuitive form. This eliminates the need to control each phase independently and instead allows unified control using vector synthesis. As a result, when the reference voltage vector rotates one revolution in the space plane, the inverter output changes one electrical cycle over time. The inverter's output frequency is equal to the rotation speed of the reference voltage vector. 


**Steps**

1) Clarke Transformation (abc → αβ)
2) Compute Reference Vector Magnitude and Angle
3) Determine Sector (1-6)
4) Switching Time Calculation
5) Switching Sequence Generation
6) Calculating Phase Duty Cycles 


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

The figure below shows the direction of the axes of the voltage in the abc reference frame and the stationary αβ reference frame.

<p align="center">
<img width="248" height="256" alt="image" src="https://github.com/user-attachments/assets/05d33308-69b1-4838-8301-e3bb7f787728" />
</p>


The figure below shows the equivalent α and β components in the stationary αβ reference frame.
<p align="center">
<img width="168" height="177" alt="image" src="https://github.com/user-attachments/assets/a4fffd60-c6a5-4ec6-8cb0-601eb883808b" />
</p>

The figure below shows the time-response of the individual components of equivalent balanced abc and αβ systems.
<p align="center">
<img width="968" height="720" alt="image" src="https://github.com/user-attachments/assets/fc29868a-6c73-4472-94f9-9edf37ab596d" />
</p>

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

The angle calculated in the above steps varies from -pi radians to +pi radians hence; it is converted to a range of 0 radians to 2pi radians. 
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

For sampling time $T_s$;


$$
T_1 = \frac{\sqrt{3}\ T_s}{V_{dc}} \ V_{ref} \sin\left(\frac{\pi}{3} - \theta_s \right)
$$

$$
T_2 = \frac{\sqrt{3}\ T_s}{V_{dc}} \ V_{ref} \sin\left(\theta_s \right)
$$

### Where:

$$
\theta_s = \theta - (sector - 1)\frac{\pi}{3}
$$

The time for zero vector is then given by;

$$
T_0 = T_s - T_1 - T_2
$$


For example, if we consider the reference vector $\vec{V}_{ref}$ and the vectors $\vec{V}_1$, $\vec{V}_2$ are shown in the figure below;

<p align="center">
<img width="400" height="347" alt="image" src="https://github.com/user-attachments/assets/6ebda4c1-7c70-4203-8d85-cfa8cca91204" />
</p>

From the above figure, the reference voltage $\vec{V}_{ref}$ is determined by the following expression,


**$\int_{0}^{T_s} \vec{V}_{ref} \, dt = \int_{0}^{T_1} \vec{V}_1 \, dt + \int_{T_1}^{T_1 + T_2} \vec{V}_2 \, dt + \int_{T_1 + T_2}^{T_s} \vec{V}_0 \, dt $**


Where T1 and T2 are lead times of the vectors V1 and V2. Since, $\vec{V}_{ref}$, $\vec{V}_1$, $\vec{V}_2$ are constants and V0 = 0, the expression becomes as shown below,

𝑉𝑉̅ref Ts= 𝑉𝑉̅ 1T1 + 𝑉𝑉̅ 2T2

