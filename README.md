

# ⏱️ Stop-Clock 


## 🎯 Objective

To design and implement a digital stopwatch with the following features:

- A **4-digit display** that shows elapsed time in the format `SS.HH`, where `SS` represents seconds and `HH` represents hundredths of a second (e.g., `23.78` indicates 23.78 seconds).
- A **single push-button interface** that cycles through three operational states: **RESET**, **START**, and **STOP**, in a sequential loop.

### Functional States:
- **RESET**: Initializes the stopwatch to `00.00`, preparing it to begin timing.
- **START**: Begins counting in 0.01-second increments. The display updates in real-time until the button is pressed again.
- **STOP**: Freezes the current time on the display. Pressing the button once more resets the stopwatch and returns it to the initial state.

## 🛠️ System Components

### 1. **Clock Generator (10 ms Pulse)**
- **IC Used**: A 555 timer is an integrated circuit used to generate precise time delays or oscillations, commonly configured three modes such as astable, monostable,and bistable modes.
- **Configuration**:In astable mode, the 555 timer functions as a free-running oscillator—it continuously switches between high and low output states without any external triggering. This makes it ideal for generating clock pulses, hence we used it for generating  10 ms clock used in our project.
- **Design Reference**: TI LM555 datasheet, Page 10.
- **Purpose**: Drives the synchronous counter to increment every 0.01 seconds.

### 2. **Debouncing Circuit**
- **Challenge**: Mechanical switches produce noisy transitions.
- **Solution**: RC filter followed by a Schmitt trigger or SR latch.
- **Simulation**: Verified using LTSpice to ensure clean, single-edge transitions.

### 3. **Synchronous BCD Counter (16-bit)**
- **ICs Used**: 74160–74163 series.
- **Design**:
  - Four 4-bit counters cascaded to form a 16-bit counter.
  - Common clock input for full synchronization.
  - Carry-look-ahead logic using ENP and ENT pins for seamless cascading.
- **Datasheet Reference**: SN54LS161A (Pages 1, 4, 11, 22).

### 4. **Display Driver**
- **Decoder**: BCD to 7-segment decoder IC.
- **Display**: Four 7-segment LED displays.
- **Format**: First two digits for seconds, last two for hundredths of a second.

---

## 🔄 State Machine Logic

```plaintext
[RESET] --(Button Press)--> [START]
[START] --(Button Press)--> [STOP]
[STOP]  --(Button Press)--> [RESET]
```

- **State Transitions**: Triggered by a single debounced push-button.
- **Control Logic**: Implemented using flip-flops or FSM logic to track state.

---

