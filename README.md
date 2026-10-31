# CAN7USAT-Precision-Landing-FSW

Official repository containing the latest GNC Precision Landing simulation suite and the unified Arduino Mega/Uno Flight Software (FSW) for **CAN-7USAT Team 017** (IN-SPACe CAN-7USAT Student Competition 2026-27).

---

## Repository Contents

### 1. `CAN-7USAT-Precision-Landing-latest/`
- Full-physics 6-DOF & ENU guidance, navigation, and control (GNC) simulation suite.
- Turn rate and dynamic loiter spiral energy management algorithms.
- Breakout planning, terminal approach, wind drift compensation, and EKF estimation routines.
- Complete Python benchmark and verification suites.

### 2. `CAN7USAT_UNO/`
- Unified C++ Flight Software supporting both **Arduino Mega 2560** and **Arduino Uno**:
  - `CAN7USAT_UNO.ino`: Main firmware entry point, serial command processor, telemetry scheduling, sensor acquisition.
  - `actuators.cpp` / `actuators.h`: Dual continuous/positional servo brake spool drivers, BLDC reaction flywheel controller, burnwire & audio buzzer MOSFET management.
  - `flight_sm.cpp` / `flight_sm.h`: 10-state deterministic flight state machine (`BOOT`, `LAUNCH_PAD`, `ASCENT`, `DROGUE_DESCENT`, `PARAFOIL_DEPLOYMENT`, `PRECISION_GUIDANCE`, `TERMINAL_GUIDANCE`, `FINAL_APPROACH`, `LANDING`, `FAILSAFE`).
  - `guidance.cpp` / `guidance.h`: 10-waypoint trajectory tracking, loiter orbit guidance, and radian-domain cross-track error PID controller.
  - `ekf.cpp` / `ekf.h`: Sensor fusion and altitude/attitude estimation.
  - `config.h`: Hardware pin assignments, loop rates, safety cutoffs, and calibration defaults.
