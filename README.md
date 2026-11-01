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
- C++ Flight Software for the **Arduino Mega 2560** (the Uno lacks the flash for the GNC code and has no SD logging, which the rules require):
  - `CAN7USAT_UNO.ino`: Main firmware entry point, serial command processor, telemetry scheduling, sensor acquisition (MS5611, MPU6050, NEO-M9N at 5 Hz UBX).
  - `gnc.cpp` / `gnc.h`: Hardware-free 10 Hz GNC step shared by the firmware and the SIL simulator.
  - `navigation.cpp` / `navigation.h`: GNSS-velocity navigation with gyro propagation (no heading drift), baro alpha-beta filter, and in-flight wind/airspeed (circle fit) and sink-rate estimation.
  - `guidance.cpp` / `guidance.h`: Homing -> loiter -> downwind leg with predicted turn-back point -> final approach with S-turns; heading controller with gyro rate loop and ground-track trim.
  - `flight_sm.cpp` / `flight_sm.h`: Flight state machine (`BOOT`, `LAUNCH_PAD`, `ASCENT`, `DROGUE_DESCENT`, `PARAFOIL_DEPLOYMENT`, `PRECISION_GUIDANCE`, `TERMINAL_GUIDANCE`, `FINAL_APPROACH`, `LANDING`, `FAILSAFE`); drone-release detection, burnwire timed so the line parts at 600 m, inflation detection.
  - `actuators.cpp` / `actuators.h`: Calibrated brake-servo mapping, BLDC flywheel ESC, burnwire & audio buzzer MOSFETs.
  - `config.h`: Pin assignments, calibration values (brake pulses, burnwire cut time, turn authority) and GNC tuning.

### 3. `sil/`
- Software-in-the-loop Monte Carlo: compiles the real `CAN7USAT_UNO` GNC code natively and flies it against an independent parafoil/wind/sensor model (`sim_world.h`).
  - `make && ./sil --n 1000` prints landing-error statistics by wind speed; `./sil --trace run.csv --index N` replays one flight. Options are listed at the top of `sil_main.cpp`.
