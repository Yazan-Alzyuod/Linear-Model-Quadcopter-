# Linear-Model-Quadcopter
# Linear Quadcopter Model (MATLAB/Simulink)

This repository contains a MATLAB script and a Simulink model designed to simulate the dynamics of a quadcopter system and generate a linearized model.

## 📂 Project Structure

The project consists of an initialization script and a Simulink model divided into specific subsystems for motor mixing, force calculation, and moment calculation.

### 1. Initialization Script (`initialization_quadcoptermodel.m`)
This script sets up the workspace parameters and performs the linearization of the model.

* **Parameters defined:**
    * [cite_start]**Inertia ($I$):** Defined with values `0.01122` (x/y axes) and `0.02199` (z axis)[cite: 4].
    * [cite_start]**Thrust Constant ($k$):** Set to $0.85 \times 10^{-5}$[cite: 4].
    * [cite_start]**Moment Constant ($b$):** Set to approximately $1.44 \times 10^{-7}$[cite: 5].
    * [cite_start]**Length ($l$):** Set to `0.23` (referred to as `1=.23` in the source)[cite: 6].
    * [cite_start]**Sampling Time ($Ts$):** Set to `0.1`[cite: 7].
* **Linearization Settings:**
    * [cite_start]**Initial State ($x$):** The state vector is initialized with 12 states, setting the altitude ($z$) to `-12`[cite: 9].
    * [cite_start]**Initial Input ($u$):** Initialized as `[0,0,0]`[cite: 10].
    * [cite_start]**Function:** Uses the `linmod` command to extract the State-Space matrices (`A, B, C, D`) from the simulink file `'LINEAR_MODEL_QUADCOBTER'`[cite: 11].
    * [cite_start]**Output:** Creates a state-space model (`quad model`) and converts it to a transfer function (`systf`)[cite: 12, 13].

### 2. Simulink Model (`LINEAR_MODEL_QUADCOBTER`)

[cite_start]The model utilizes a "6DOF (Euler Angles)" block to simulate rigid body dynamics [cite: 86-117]. It processes inputs through the following subsystems:

#### **SubSystem 1: Motor Mixing**
* [cite_start]**Inputs:** `thrust`, `pitch`, `roll cmd`, `yaw cmd` [cite: 14-20].
* **Operation:** Mixes these command signals using summation points to generate individual motor commands.
* [cite_start]**Outputs:** `motor1`, `motor2`, `motor3`, `motor4` [cite: 22-29].

#### **SubSystem 2: Force Calculation ($F_{xyz}$)**
* **Inputs:**
    * [cite_start]`DCM` (Direction Cosine Matrix)[cite: 45].
    * [cite_start]`motor1` through `motor4` (squared inputs $u^2$) [cite: 53-64].
* **Logic:**
    * Calculates the total thrust by summing the squared motor inputs multiplied by gain `k`.
    * [cite_start]Incorporates a vector `[0, 0, 65]` processed through a Gain block (`-K-`) and multiplied by the `DCM` via a Matrix Multiply block [cite: 46-51, 65].
    * Sums the thrust and weighted vector to produce the final force.
* [cite_start]**Outputs:** `Fxyz` (Force vector)[cite: 67].

#### **SubSystem 3: Moment Calculation ($M_{xyz}$)**
* [cite_start]**Inputs:** `motor1` through `motor4`[cite: 30].
* **Logic:**
    * [cite_start]Squares the motor inputs ($u^2$) [cite: 31-34].
    * [cite_start]Uses Sum blocks to compute differences between motor pairs [cite: 35-38].
    * [cite_start]Applies gains labeled `I*k` to the roll/pitch channels[cite: 39, 41].
    * Applies gain `b` to the yaw channel.
* [cite_start]**Outputs:** `Mxyz` (Moment vector)[cite: 43].

## 🚀 Usage

1.  Open MATLAB.
2.  Run `initialization_quadcoptermodel.m` to load parameters ($k, b, l, I$) and generate the linear model matrices.
3.  Open the Simulink file (referenced as `LINEAR_MODEL_QUADCOBTER` in the script) to view or run the simulation.# Linear Quadcopter Model (MATLAB/Simulink)

This repository contains a MATLAB script and a Simulink model designed to simulate the dynamics of a quadcopter system and generate a linearized model.

## 📂 Project Structure

The project consists of an initialization script and a Simulink model divided into specific subsystems for motor mixing, force calculation, and moment calculation.

### 1. Initialization Script (`initialization_quadcoptermodel.m`)
This script sets up the workspace parameters and performs the linearization of the model.

* **Parameters defined:**
    * [cite_start]**Inertia ($I$):** Defined with values `0.01122` (x/y axes) and `0.02199` (z axis)[cite: 4].
    * [cite_start]**Thrust Constant ($k$):** Set to $0.85 \times 10^{-5}$[cite: 4].
    * [cite_start]**Moment Constant ($b$):** Set to approximately $1.44 \times 10^{-7}$[cite: 5].
    * [cite_start]**Length ($l$):** Set to `0.23` (referred to as `1=.23` in the source)[cite: 6].
    * [cite_start]**Sampling Time ($Ts$):** Set to `0.1`[cite: 7].
* **Linearization Settings:**
    * [cite_start]**Initial State ($x$):** The state vector is initialized with 12 states, setting the altitude ($z$) to `-12`[cite: 9].
    * [cite_start]**Initial Input ($u$):** Initialized as `[0,0,0]`[cite: 10].
    * [cite_start]**Function:** Uses the `linmod` command to extract the State-Space matrices (`A, B, C, D`) from the simulink file `'LINEAR_MODEL_QUADCOBTER'`[cite: 11].
    * [cite_start]**Output:** Creates a state-space model (`quad model`) and converts it to a transfer function (`systf`)[cite: 12, 13].

### 2. Simulink Model (`LINEAR_MODEL_QUADCOBTER`)

[cite_start]The model utilizes a "6DOF (Euler Angles)" block to simulate rigid body dynamics [cite: 86-117]. It processes inputs through the following subsystems:

#### **SubSystem 1: Motor Mixing**
* [cite_start]**Inputs:** `thrust`, `pitch`, `roll cmd`, `yaw cmd` [cite: 14-20].
* **Operation:** Mixes these command signals using summation points to generate individual motor commands.
* [cite_start]**Outputs:** `motor1`, `motor2`, `motor3`, `motor4` [cite: 22-29].

#### **SubSystem 2: Force Calculation ($F_{xyz}$)**
* **Inputs:**
    * [cite_start]`DCM` (Direction Cosine Matrix)[cite: 45].
    * [cite_start]`motor1` through `motor4` (squared inputs $u^2$) [cite: 53-64].
* **Logic:**
    * Calculates the total thrust by summing the squared motor inputs multiplied by gain `k`.
    * [cite_start]Incorporates a vector `[0, 0, 65]` processed through a Gain block (`-K-`) and multiplied by the `DCM` via a Matrix Multiply block [cite: 46-51, 65].
    * Sums the thrust and weighted vector to produce the final force.
* [cite_start]**Outputs:** `Fxyz` (Force vector)[cite: 67].

#### **SubSystem 3: Moment Calculation ($M_{xyz}$)**
* [cite_start]**Inputs:** `motor1` through `motor4`[cite: 30].
* **Logic:**
    * [cite_start]Squares the motor inputs ($u^2$) [cite: 31-34].
    * [cite_start]Uses Sum blocks to compute differences between motor pairs [cite: 35-38].
    * [cite_start]Applies gains labeled `I*k` to the roll/pitch channels[cite: 39, 41].
    * Applies gain `b` to the yaw channel.
* [cite_start]**Outputs:** `Mxyz` (Moment vector)[cite: 43].
## 👨‍💻 Author
**Yazan Alzyuod**
* 📧 [yqlasem@gmail.com](mailto:yqlasem@gmail.com)
* 🔗 [LinkedIn Profile](https://www.linkedin.com/in/yazan-alzyuod)
* 💻 [GitHub Profile](https://github.com/Yazan-Alzyuod)
* 📞 00962775327776
