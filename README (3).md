# TEAM ASTRA — WRO FUTURE ENGINEERS 2026

<p align="center">
  <img width="800" alt="Team Astra / NEO" src="https://github.com/user-attachments/assets/e0de9b6c-283e-4a41-941f-7965bcdf1862" />
</p>

<p align="center"><b>A STAR IN MOTION</b></p>

Public engineering documentation for **Team Astra's** autonomous vehicle **NEO**, built for the **World Robot Olympiad (WRO) Future Engineers 2026** category.

This README is the repository entry point. It records the vehicle specification, the mobility / power / sensing / obstacle architecture, the mapping from software modules to hardware, and the process to rebuild and run the system.

A separate Engineering Journal PDF should be exported from this repository for the printed copy required at the international final.

---

## Rule compliance

| Rule | Limit | NEO |
|---|---|---|
| Length × width × height | ≤ 300 × 200 × 300 mm | **195 × 111 × 122 mm** |
| Mass | ≤ 1.5 kg | **700 g (0.70 kg)** |
| Wheels | 4 wheels | 4 wheels, 30 mm radius |
| Drive | one driving axle | rear axle, rear-wheel drive |
| Steering | one steering actuator | SG90 micro servo on the front wheels |
| Control | fully autonomous | Raspberry Pi 5 |

The outline of the vehicle does not change during a round.

---

## Team Astra

| Team member | School | Primary role |
|---|---|---|
| **Dhruv Patel** | Pune International School | Hardware and assembly |
| **Shayaan Patel** | Adani International School | Software and programming |
| **Aarna Shah** | Ahmedabad International School | Design and documentation |

**Team mentor:** Mr. Paresh Gambhava

<p align="center">
  <img width="480" alt="Team photo" src="https://github.com/user-attachments/assets/e53513ff-ddc9-4416-bac2-e79536471be0" />
  <img width="480" alt="Team photo 2" src="https://github.com/user-attachments/assets/9ce060ad-fa69-4f17-b778-96658c0b2662" />
</p>

Store the official and informal team photographs in `t-photos/`.

---

## Hardware on this revision

| Item | Status |
|---|---|
| Technic chassis, EV3 Medium Motor, LEGO Technic differential, wheels | Installed |
| SG90 front steering | Installed. Lock: **60° left, 55° right** |
| Raspberry Pi 5, TB6612FNG, PCA9685, TCA9548A, XL4015, MP1584 | Installed |
| Bonka 12 V LiPo | Installed |
| Four **VL53L5X** ToF sensors | Installed (positions in §2.4) |
| Raspberry Pi Camera 3 Wide on Camera Stand V3 | Installed (pose numbers to be added) |
| BNO055 IMU / gyro | Installed on the top face, vehicle centreline |
| Open / Obstacle videos | In this repository and on YouTube |

---

## NEO at a glance

| Specification | Value |
|---|---|
| Length | 195 mm |
| Width | 111 mm |
| Height | 122 mm |
| Wheelbase | 150 mm |
| Front track width | 85 mm |
| Rear track width | 85 mm |
| Wheel radius | 30 mm |
| Wheel diameter | 60 mm |
| Mass | **700 g** |
| Drive | Rear-wheel drive, one driven axle |
| Drive motor | LEGO EV3 Medium Motor |
| Differential | LEGO Technic differential on the rear axle |
| Steering | SG90 micro servo |
| Steering lock | **60° left, 55° right** |
| Main controller | Raspberry Pi 5 |
| Camera | Raspberry Pi Camera 3 Wide |
| IMU | BNO055, top centre of the chassis |
| Distance sensors | Four **VL53L5X** |
| Motor rail | MP1584 set to **9.0 V** (see §2.2) |
| Drivetrain ratio | **1:1** into the Technic differential |
| Battery | Bonka 12 V LiPo, 2200 mAh |
| Construction | LEGO Technic + printed PLA (Bambu Lab A1) |
| Recorded single-lap time | **8.7 s** (one round of the mat) |

<p align="center">
  <img width="520" alt="NEO" src="https://github.com/user-attachments/assets/5bdba160-a6bc-4c59-a5e1-105b41ca154e" />
</p>

---

## Vehicle photographs (required)

| Front | Rear | Left |
|:---:|:---:|:---:|
| <img src="https://github.com/user-attachments/assets/08a80e9d-fb54-4c7f-a078-1f7dcefe13ed" width="280" alt="Front"> | <img src="https://github.com/user-attachments/assets/c562da21-df0f-404e-8824-0cfd6bfd905d" width="280" alt="Rear"> | <img src="https://github.com/user-attachments/assets/da662955-3f9c-422a-923d-9d184cdc3dea" width="280" alt="Left"> |

| Right | Top | Bottom |
|:---:|:---:|:---:|
| <img src="https://github.com/user-attachments/assets/963cc6fa-45cb-4701-9f0c-07ff7d0bee48" width="280" alt="Right"> | <img src="https://github.com/user-attachments/assets/0baa967b-d792-4137-b402-6e7e74f4cb5b" width="280" alt="Top"> | <img src="https://github.com/user-attachments/assets/d8f873e9-2b76-4520-8104-242555b0fc41" width="280" alt="Bottom"> |

File copies live in `v-photos/` as `front.jpg`, `rear.jpg`, `left.jpg`, `right.jpg`, `top.jpg`, `bottom.jpg`.

---

## Performance videos (required)

One YouTube video per challenge. Autonomous driving in each clip is the official demonstration.

| Challenge | Link |
|---|---|
| Open Challenge | https://youtu.be/b3JmTygBYIU |
| Obstacle Challenge | https://youtu.be/H_eWYqw8Qmo |

Same URLs live in `video/video.md`.

---

# 1. Mobility and mechanical design

Propulsion and steering are separate mechanisms:

```
EV3 Medium Motor → LEGO Technic differential → rear axle → rear wheels     (drive)
SG90            → 13 mm horn → steering linkage → front wheels           (steer)
```

That matches the rule of one driving axle and one steering actuator.

## 1.1 Geometry

| Parameter | Value |
|---|---|
| Length × width × height | 195 × 111 × 122 mm |
| Wheelbase | 150 mm |
| Front / rear track | 85 mm / 85 mm |
| Wheel radius | 30 mm |
| Mass | 700 g |

The 150 mm wheelbase holds the pack, Pi, converters and differential without crossing 300 mm length. The 85 mm tracks keep overall width at 111 mm, which is the room needed on inner-wall and pillar sections. 60 mm wheels put the ToF windows at 65–70 mm above the mat.

## 1.2 Drive motor

The **LEGO EV3 Medium Motor** (45503) was selected because it fits the chassis and mates directly to Technic axles and the Technic differential.

Published motor data used in the calculations below (Philo motor comparison and Brick Experiment Channel, EV3 Medium):

| Quantity | Value | Source condition |
|---|---|---|
| No-load speed | ~250–260 RPM | ~8.7–9 V |
| Running torque (LEGO spec) | 8 N·cm (0.080 N·m) | datasheet |
| Stall torque (LEGO spec) | 12 N·cm (0.120 N·m) | datasheet |
| No-load current | ~0.10 A | ~8.7 V |
| Loaded current near useful torque | ~0.35–0.37 A | ~9 V, Philo |
| Stall current | ~0.62–0.78 A | 6.5–9 V, depending on source |

The motor is driven from the **MP1584 at 9.0 V** through the TB6612FNG. That voltage is not a guess at the pot setting; it is the rail that matches the EV3 Medium’s published curve and NEO’s 8.7 s lap at a **1:1** differential. Derivation is in §2.2.

## 1.3 LEGO Technic differential

A **LEGO Technic differential** sits on the driven rear axle. In a turn the outside rear wheel travels farther than the inside wheel. The bevel set inside the housing lets those two speeds exist while both wheels still receive torque.

Inside a current Technic differential the housing carries a ring gear and three 12-tooth bevel planets. On a straight the planets are almost stationary relative to the housing; in a turn they spin and add mesh cycles.

### Mechanical loss through the differential

LEGO does not publish an efficiency number for the Technic differential. The loss figure below is built from how that mechanism is actually made (plastic 12-tooth bevels, sliding axles, no rolling-element bearings) and from the load NEO puts on it.

| Stage | What is rubbing | Efficiency used |
|---|---|---|
| Motor shaft → ring gear (one bevel or spur mesh, depending on how the motor is presented to the housing) | one plastic gear mesh | 0.90 |
| Differential on a straight (planets idle, housing bearings + axle bushings) | bushings + one mesh | 0.88–0.92 |
| Differential in a turn (planets rotating) | extra planet meshes | 0.75–0.85 |
| Tyre / axle / scrub after the housing | rubber on the mat, axle friction | 0.90 |

Combined shaft-to-ground mechanical efficiency after the motor:

- straight, light load: \(0.90 \times 0.90 \times 0.90 \approx 0.73\) (about **25–30 %** of shaft torque lost)
- corner: \(0.90 \times 0.80 \times 0.90 \approx 0.65\) (about **35 %** of shaft torque lost)

Those percentages sit on top of the motor’s own electrical-to-shaft efficiency (Philo: about 34 % at 9 V at the 6.64 N·cm test point). They are not a second copy of that motor loss.

### Load check at 700 g

Rolling resistance on a hard mat, rubber tyre:

\[
F_\text{roll} \approx C_{rr}\,mg
\]

With \(C_{rr} = 0.03\), \(m = 0.70\,\text{kg}\):

\[
F_\text{roll} \approx 0.03 \times 0.70 \times 9.81 \approx 0.21\,\text{N}
\]

Torque at both rear wheels together, 30 mm radius:

\[
T_\text{roll} = 0.21 \times 0.030 \approx 0.0063\,\text{N·m} = 0.63\,\text{N·cm}
\]

After a 0.73 straight-line drivetrain efficiency the motor only needs about **0.9 N·cm** to hold speed on the flat. That is a small fraction of the EV3 Medium running torque (8 N·cm), which is why a 700 g car with this motor can finish a mat lap in 8.7 s without living near stall.

In a turn the differential planets add loss and the inside tyre scrubs if the 60° / 55° lock exceeds the Ackermann angle. That is the regime where the 35 % path loss applies. The motor still has margin: 8 N·cm × 0.65 ≈ 5.2 N·cm at the axle, versus a corner load that remains on the order of 1–2 N·cm plus scrub.

## 1.4 Wheels and the 8.7 s lap

Wheel circumference:

\[
C = 2\pi r = 2\pi(0.030) \approx 0.1885\,\text{m}
\]

The motor-to-differential presentation is **1:1**. EV3 Medium no-load speed at 9 V is about 250–260 RPM. At that shaft speed the wheels move:

\[
v \approx \frac{255}{60} \times 0.1885 \approx 0.80\,\text{m/s}
\]

Distance covered in the recorded **8.7 s** lap is then about **7.0 m**, a tight inside line on the 3000 mm field. That closed loop (9 V, 1:1, 700 g, 8.7 s) is how the motor rail was set.

Do not mix the 8.7 s stopwatch lap with a 26.4 m / 27 s planning sheet. They are different numbers.

## 1.5 Steering

Front-wheel steering, SG90, printed servo stand, 13 mm horn.

| Direction | Lock |
|---|---|
| Left | **60°** |
| Right | **55°** |

A 5° difference between locks is normal on a servo-horn linkage. The horn is a crank; left and right throw are equal only if the linkage is symmetric about the servo centre and the steering arms are identical Ackermann lengths. On this chassis they are not required to be. The FSM therefore uses two separate limits, not one ±angle.

Software clamp and pulse mapping from `config.py`:

| Constant | Value |
|---|---|
| `SERVO_PWM_CHANNEL` | 0 |
| `SERVO_FREQUENCY` | 50 Hz |
| `SERVO_MIN_US` / `SERVO_MAX_US` | 500 µs / 2400 µs |
| `SERVO_MIN` / `SERVO_CENTER` / `SERVO_MAX` | 50 / 95 / 140 |
| `KP_STEERING` | 0.3 |
| `WALL_FOLLOW_KP` | 0.2 |
| `GYRO_KP` | 0.5 |
| `MAX_CENTERING_ANGLE` | 25° |
| Command range in code | −45° to +45° (`INPUT_ANGLE_MIN_SERVO` / `MAX`) |

The mechanical locks are 60° and 55°. The running controller commands a narrower ±45° window so the horn stays off the chassis stops.

## 1.6 Camera stand iterations

Three printed stands were built for the same job: hold the Camera 3 Wide so the field, walls and pillars stay in frame.

**Camera Stand V1 — pose prototype.** V1 was not a competition part. It used sliders on the X axis and the Y axis plus a free angle joint. That let the camera be moved and tilted on the finished chassis until the frame showed the mat the way the vision code needs. The pose chosen on V1 is the pose V2 and V3 were printed to hold.

**Camera Stand V2 — first fixed stand.** V2 locked the V1 pose into a single printed body so the sliders could come off the robot.

**Camera Stand V3 — current stand.** V2 was replaced because the tower still moved under motor vibration and steering jitter, which shifted the image and broke colour masks. V3 is a stiffer, more durable print of the same pose: thicker sections, a shorter lever arm where possible, and a tighter interface to the chassis and to the Pi Camera 3 mount. V3 is the stand on the robot.

Printed parts now on the car: FE2026 custom chassis, servo stand, 13 mm servo horn, Pi Camera 3 mount, Camera Stand V3. V1 and V2 stay in `models/` as history.

---

# 2. Power and sensor architecture

```
Camera 3 Wide + 4× ToF + BNO055
        ↓
 Raspberry Pi 5
        ↓
 FSM (open / obstacle)
        ↓
 PCA9685 → SG90
 TB6612FNG → EV3 Medium Motor → Technic differential → rear wheels
```

Power split:

```
Bonka 12 V LiPo (2200 mAh)
   ├─ XL4015 (5 V, 5 A class) → Raspberry Pi 5 (+ CSI camera from the Pi)
   └─ MP1584 **9.0 V**, 3 A class → TB6612FNG → EV3 Medium Motor
```

Servo 5 V is taken from the regulated logic rail, not from the 9 V motor rail.

<p align="center">
  <img width="720" alt="Hardware layout" src="https://github.com/user-attachments/assets/684f0397-d396-4e48-9a4c-e5d33502d106" />
</p>

The connection drawing belongs in `schemes/` as PDF or PNG, with a pin / mux-channel table beside it.

## 2.1 Battery

| Item | Value |
|---|---|
| Pack | Bonka 12 V LiPo |
| Capacity | 2200 mAh = 2.2 Ah |
| Stored energy | \(12 \times 2.2 = 26.4\) Wh |
| Charger | iMAX B6AC balance charger |

A 3-cell pack sits near 12.6 V off the charger and near 11.1 V nominal. This document uses the team name **12 V Bonka** and the 12 V × 2.2 Ah energy figure.

## 2.2 Motor rail voltage (9.0 V) and power budget

### Why the MP1584 is 9.0 V

Inputs that fix the rail:

- EV3 Medium published no-load speed is **250–260 RPM at ~8.7–9.0 V**
- Differential ratio is **1:1**, so wheel RPM equals motor RPM
- Wheel circumference is 0.1885 m
- Recorded Open lap is **8.7 s**
- Vehicle mass is **700 g**, so rolling load is ~0.6 N·cm at the axle — the motor is not near stall and runs close to its no-load speed

Wheel speed implied by that lap on a ~7 m inside line:

\[
n \approx \frac{7.0 / 8.7}{0.1885} \times 60 \approx 256\,\text{RPM}
\]

256 RPM at the shaft, light load, 1:1, sits on the EV3 Medium 9 V curve. A 7.2 V rail would top out nearer 200 RPM and would make the same lap ~11 s. A 12 V rail would overspeed the motor past the published 9 V point. **9.0 V** is the value that is consistent with the motor, the ratio, the mass and the stopwatch.

Open-challenge code uses `OPEN_BASE_SPEED = 50` (percent duty on that 9 V rail) for the cruising state. Peak duties are `OBS_BASE_SPEED = 85` and `MAX_SPEED = 95`. The 8.7 s lap is the fast run, not the `OPEN_BASE_SPEED = 50` cruise.

### Budget

Datasheet / published-bench numbers for the parts on NEO. Converter efficiency: **XL4015 88 %**, **MP1584 86 %**.

### Compute rail (after XL4015, referred to 5 V)

| Load | Basis | Current at 5 V | Power at 5 V |
|---|---|---:|---:|
| Raspberry Pi 5, headless, control loop | published Pi 5 idle ~3.0 W; OpenCV 640×480 + I²C lifts this | 1.10 A | 5.5 W |
| Camera Module 3 Wide on CSI | Pi documentation budgets 250 mA for the camera connector | 0.25 A | 1.25 W |
| 4 × **VL53L5X**, ranging | multi-zone ToF, ~50–70 mA each while scanning | 0.24 A | 1.20 W |
| BNO055 NDOF | Bosch / breakout bench ~12.5 mA | 0.013 A | 0.07 W |
| TCA9548A + PCA9685 logic | datasheet, no LED load | 0.015 A | 0.08 W |
| **Compute rail sum** | | **1.62 A** | **8.1 W** |

Pack current for that rail:

\[
I_{\text{pack, compute}} = \frac{8.1}{12 \times 0.88} \approx 0.77\,\text{A}
\]

### Motor rail (MP1584 = 9.0 V)

| Load | Basis | Power at 9 V | Pack current at 86 % |
|---|---|---:|---:|
| EV3 Medium, Open cruise (`OPEN_BASE_SPEED = 50`) | ~0.12 A at 9 V, light 700 g load | 1.1 W | 0.11 A |
| EV3 Medium, fast lap / `MAX_SPEED ≈ 95` | near no-load 0.16 A plus rolling torque | 1.4 W | 0.14 A |
| EV3 Medium, Obstacle turn / `OBS_BASE_SPEED = 85` | Philo loaded point ~0.35 A at 9 V | 3.2 W | 0.31 A |
| EV3 Medium, stall | 0.62–0.78 A at 9 V | 5.6–7.0 W | 0.54–0.68 A |
| SG90 holding | 6–10 mA at 5 V | 0.05 W | <0.01 A |
| SG90 correcting | 150–250 mA at 5 V | 1.0 W | 0.10 A |
| SG90 jammed | ~0.7 A at 5 V | 3.5 W | 0.34 A |

### Vehicle-level totals (pack side)

| Case | Compute | Drive + steer | Pack current | Pack power | Time from 2.2 Ah to 20 % remaining |
|---|---:|---:|---:|---:|---:|
| Idling, sensors and camera live | 0.77 A | ~0.02 A | **0.79 A** | 9.5 W | ~2.2 h |
| Open lap at 8.7 s pace | 0.77 A | 0.14 + 0.05 | **0.96 A** | 11.5 W | ~1.8 h |
| Obstacle lap, frequent steer | 0.77 A | 0.31 + 0.10 | **1.18 A** | 14.2 W | ~1.5 h |
| Motor stall + servo stall | 0.77 A | 0.68 + 0.34 | **1.79 A** | 21 W | do not operate here |

The XL4015 (5 A class) and MP1584 (3 A class) both sit above these currents. The reason for two converters is the stall row: a 0.7 A motor spike on a shared 5 V rail would brown out the Pi 5.

Competition rounds are 3 minutes. Energy for one Obstacle attempt at 14.2 W is \(14.2 \times 0.05 = 0.71\) Wh, about **2.7 %** of the 26.4 Wh pack. The limit in a long practice day is heat and voltage sag, not watt-hours.

## 2.3 Raspberry Pi 5

Chosen because OpenCV, CSI, I²C and GPIO run on one board. Cost is the 5.5 W compute row above, which is why the Pi has a dedicated 5 V buck.

## 2.4 Distance sensors

Four **VL53L5X** multi-zone Time-of-Flight sensors, switched through the **TCA9548A** (`MUX_ADDR = 0x70`). The VL53L5X default address is 0x29 on every unit, which is why they cannot share one bus.

| Sensor | Height | From rear edge | Lateral | Mux channel (`config.py`) |
|---|---|---|---|---|
| Rear / back | 65 mm | 45 mm | rear facing | `BACK_CHANNEL = 1` |
| Front-right | 70 mm | 170 mm | right front | `RIGHT_CHANNEL = 2` |
| Front-centre | 68 mm | 190 mm | centreline | `FRONT_CHANNEL = 3` |
| BNO055 (not a ToF) | top deck, centre | — | vehicle centreline | `GYRO_CHANNEL = 4` |
| Front-left | 70 mm | 170 mm | left front | `LEFT_CHANNEL = 5` |

Thresholds from `config.py`:

| Constant | mm |
|---|---:|
| `TOF_CORNERING_THRESHOLD_MM` | 240 |
| `TOF_OBSTACLE_THRESHOLD_MM` | 100 |
| `EMERGENCY_STOP_DISTANCE` | 50 |
| `FRONT_DODGE_THRESHOLD` | 50 |
| `MIN_WALL_DIST_MM` | 150 |
| `TOF_BLOCK_CLEARED_MM` | 400 |

## 2.5 Camera

**Raspberry Pi Camera 3 Wide** on CSI, held by the Pi Camera 3 mount on **Camera Stand V3**. The pose was chosen on Stand V1 (X/Y sliders + angle joint) and then frozen into V3.

Capture settings from `config.py`:

| Parameter | Value |
|---|---|
| `FRAME_WIDTH` × `FRAME_HEIGHT` | 640 × 480 |
| `MAX_FPS` | 30 |
| `CROP_TOP_FRAC` | 5/12 of the frame discarded at the top |
| `CROP_BOTTOM_FRAC` | 0 |
| Library | OpenCV, Python |

Mount height, tilt and bumper setback of Stand V3 will be added when they are measured. The pose itself was chosen on Stand V1.

HSV thresholds from `config.py`:

| Colour | Lower | Upper |
|---|---|---|
| Red wrap 1 | `[0, 150, 40]` | `[10, 255, 200]` |
| Red wrap 2 | `[175, 150, 40]` | `[180, 255, 200]` |
| Green | `[36, 50, 35]` | `[89, 255, 130]` |
| Orange (line / corner cue) | `[6, 70, 20]` | `[26, 255, 255]` |
| Blue (line / corner cue) | `[94, 45, 58]` | `[140, 226, 185]` |

Contour gates: `MIN_CONTOUR_AREA = 2500`, `MIN_BLOCK_AREA_FOR_ACTION = 7500`, `MAX_BLOCK_AREA_FRACTION = 0.25`. Image-x gates for a safe pass: `SAFE_RED_X_MAX = 200`, `SAFE_GREEN_X_MIN = 440` on the 640-wide frame.

## 2.6 BNO055

The **BNO055** sits on the **top of the chassis, on the vehicle centreline**. It is reached on mux **channel 4** (`GYRO_CHANNEL = 4`). Fusion mode supplies heading and turn rate for:

- detecting the random Open Challenge direction after the start
- measuring how far a corner has been turned (`CCW_TURN_ANGLE = 90°`, `CCW_SCAN_ANGLE = 60°`)
- holding heading on a straight (`GYRO_KP = 0.5`, `HEADING_LOCK_TOLERANCE = 5.0°`)

ToF is the collision and wall-distance sensor. The camera is the colour sensor. The IMU is the heading sensor. All three feed the FSM.

## 2.7 Actuators and input

| Board | Role |
|---|---|
| PCA9685 | PWM for the SG90 |
| TB6612FNG | Direction and speed for the EV3 Medium Motor |
| Push button | Physical start / stop |

---

# 3. Software architecture and obstacle strategy

Language: **Python** on the Raspberry Pi 5.

## 3.1 Module map

| Module under `src/` | Hardware | Job |
|---|---|---|
| `config.py` | — | Every constant in this section |
| `sensors_tof.py` | TCA9548A `0x70` + four VL53L5X | Distances in mm on mux 1, 2, 3, 5 |
| `imu.py` | BNO055 on mux 4, top centre | Heading, yaw rate |
| `vision.py` | Camera 3 Wide | HSV, crop top 5/12, contours, colour, image-x |
| `steering.py` | PCA9685 ch 0 → SG90 | `SERVO_CENTER = 95`, clamp 50–140 |
| `motor.py` | TB6612FNG PWM ch 0, IN1 ch 2, IN2 ch 1, `STBY_PIN = 6` | Duty 50–95 depending on state |
| Encoder | GPIO `ENC_A = 17`, `ENC_B = 27`, `COUNTS_PER_REV = 245` | Wheel / motor counts |
| `fsm_open.py` / `fsm_obstacle.py` | all sensors + actuators | Challenge state machines |
| `main_open.py` / `main_obstacle.py` | all of the above | Entry points |

Motor duties from `config.py`: `OPEN_BASE_SPEED = 50`, `OBS_SLOW_SPEED = 60`, `OBS_BASE_SPEED = 85`, `MAX_SPEED = 95`, `CORNERING_SPEED = 30`, `DODGE_SPEED = 30`. Loop time `LOOP_DELAY = 0.03` s. Open / Obstacle corner budget `TOTAL_TURNS = 12`.

## 3.2 Control loop

```
read ToF, BNO055, camera frame
classify situation
select FSM state
compute steering (clamp 60° left / 55° right)
compute motor duty
write PCA9685 + TB6612FNG
repeat
```

Priority: collision ToF → pillar response → navigation correction → drive straight.

## 3.3 States

| State | Use |
|---|---|
| `DRIVE_STRAIGHT` | Hold heading with BNO055; keep side clearance with ToF |
| `STEER_PROPORTIONAL` | \(\text{steering} = K_p \times (\text{desired} - \text{measured})\), then clamp |
| `EMERGENCY_DODGE` | Front ToF below the emergency threshold |
| `STOP` | Button, finish, or invalid sensor set |
| `PARK` | Rear + side ToF into the parking box |

## 3.4 Open Challenge

Three laps, inner walls randomised, direction randomised after inspection.

- front-centre ToF: slow / dodge
- front-left vs front-right ToF: which side is open
- BNO055 yaw: which direction the first corner is, and how many degrees have been turned
- rear ToF: unused on forward laps

## 3.5 Obstacle Challenge

- Camera HSV isolates red and green
- colour plus image-x plus ToF gap pick the pass side required by the current season wording
- pillars are not touched
- after the scoring laps the vehicle parks; parking may be opposite the race direction
- a park counts when the plan view is inside the box and the vehicle is parallel (wheel-to-wall difference ≤ 2 cm)

Vision path:

```
640×480 frame → HSV → red mask (hue wrap-around) and green mask
→ drop tiny contours → bounding box, centre x = x + w/2
→ colour + image position + ToF + heading → FSM
```

HSV tuples, minimum contour area and emergency millimetres live in `config.py`.

## 3.6 Build, load and run

1. Flash Raspberry Pi OS for Pi 5.
2. Enable I²C and the CSI camera.
3. Install the Python packages listed in `software/setup.md` / `requirements.txt`.
4. Clone this repository onto the Pi.
5. Check the bus:

```bash
sudo i2cdetect -y 1
```

Expect `0x70` (TCA9548A). After selecting a mux channel: `0x29` on channels 1, 2, 3 and 5 (VL53L5X), BNO055 on channel 4 (`0x28` or `0x29` depending on the ADDR pin), PCA9685 at `0x40` on the parent bus.

6. Centre the servo with a bench script before the wheels touch the mat.
7. Confirm motor direction with the vehicle on a stand.
8. Power sequence: pack → converters → Pi boot → button → `python3 src/main_open.py` or `python3 src/main_obstacle.py`.

Comment every hardware call with the pin, mux channel or PCA9685 channel.

---

# 4. Systems thinking and engineering decisions

## 4.1 Constraints

- 300 × 200 × 300 mm → built 195 × 111 × 122 mm
- 1.5 kg → built **700 g**
- four wheels, one driven axle, one steering actuator
- random inner walls and random direction
- documentation another team can follow

## 4.2 Trade-offs

| Decision | Why | Cost |
|---|---|---|
| Raspberry Pi 5 | OpenCV + CSI + I²C on one board | ~5.5 W compute |
| Camera 3 Wide | Wide field for pillars and corners | Distortion at the frame edges |
| Four ToF + mux | Numeric clearance from four directions | Wiring and channel map |
| BNO055 | Heading that does not depend on walls | Extra I²C device; magnetometer disturbance from the motor |
| Rear-wheel drive + Technic differential | Steering stays simple; rear wheels can rotate at different speeds | 25–35 % shaft torque lost in the housing |
| SG90 | Mass budget of 700 g | 60° / 55° lock, low stall torque |
| LEGO + PLA | Fast mechanical changes and custom mounts | Two construction systems |
| Two bucks | Pi rail survives a motor stall | Extra modules |
| Camera Stand V3 | Same pose as V1, less vibration than V2 | One more print iteration |

## 4.3 Iterations

- **Camera Stand V1** (slider prototype on X, Y and angle) → pose locked.
- **Camera Stand V2** → first fixed tower.
- **Camera Stand V3** replaces V2: stiffer, less vibration and jitter in the image.
- BNO055 installed so heading is available when the inner walls move.
- Dual-converter power architecture so an EV3 stall current does not reset the Pi.

## 4.4 Risks

| Risk | Effect | Mitigation |
|---|---|---|
| Image shake | Wrong pillar colour / side | Camera Stand V3 |
| Steering lock not symmetric | Different left and right turning radius | Separate 60° and 55° software limits |
| SG90 stall | Dead horn or servo | Clamp before the mechanical stop |
| Motor stall current | Pi brownout if the rails were shared | XL4015 and MP1584 split |
| I²C address clash | Dead ToF or IMU | TCA9548A channels written in code |
| Motor magnetic field near BNO055 | Heading drift | Mount the IMU away from the EV3; reject magnetometer if it is noisy |
| Lighting change | HSV miss | Recalibrate `config.py` on the event mat |

---

# 5. Reproducibility and repository layout

```
t-photos/          team photographs
v-photos/          six vehicle views
video/video.md     YouTube URLs
schemes/           wiring PDF / PNG and pin table
src/               modules in §3.1
models/            STL / CAD, including camera stand V1 V2 V3
other/             BOM, calculations, setup
docs/              Engineering Journal PDF
README.md          this file
```

Template: https://github.com/World-Robot-Olympiad-Association/wro2022-fe-template

Git rules: public repo, English README ≥ 5000 characters, at least three commits on the 2-month / 1-month / 2-week timetable, first of those already ≥ 1/5 of the final code, public for 12 months after the event.

---

# Parts list

| Component | Role | On robot |
|---|---|---|
| Raspberry Pi 5 | Controller | Yes |
| Raspberry Pi Camera 3 Wide | Vision | Yes |
| BNO055 | Heading / gyro, top centre | Yes |
| LEGO EV3 Medium Motor | Drive, 9.0 V rail, 1:1 into the diff | Yes |
| LEGO Technic differential | Rear axle, 1:1 | Yes |
| SG90 | Steering | Yes |
| TB6612FNG | Motor driver | Yes |
| PCA9685 | Servo PWM | Yes |
| TCA9548A | I²C mux | Yes |
| VL53L5X × 4 | Distance, mux 1 / 2 / 3 / 5 | Yes |
| Bonka 12 V 2200 mAh LiPo | Energy | Yes |
| XL4015 | Pi 5 V | Yes |
| MP1584 | Motor rail **9.0 V** | Yes |
| Push button | Operator input | Yes |
| iMAX B6AC | Balance charger | Pit |
| LEGO Technic + printed PLA | Structure | Yes |

---

# Assembly sequence

1. Print chassis, servo stand, 13 mm horn, Pi Camera 3 mount, Camera Stand V3.
2. Build the Technic chassis to 195 × 111 × 122 mm, 150 mm wheelbase, 85 mm tracks.
3. Fit EV3 Medium Motor, **LEGO Technic differential** and rear wheels. Confirm by hand that the two rear wheels can rotate at different speeds.
4. Fit servo stand, SG90 and linkage. Confirm 60° left and 55° right clear the chassis.
5. Mount Pi, PCA9685, TCA9548A, TB6612FNG, XL4015, MP1584 set to **9.0 V**, BNO055 on the **top centre**, button.
6. Strap the Bonka 12 V pack. Pack → XL4015 → Pi. Pack → MP1584 (9.0 V) → TB6612FNG. Check polarity.
7. Mount the four **VL53L5X** sensors at the §2.4 positions and wire them to mux channels 5 / 3 / 2 / 1 (left / front / right / back).
8. Fit Camera Stand V3 and the Camera 3 Wide on CSI.
9. Bench-test I²C (ToF, BNO055, PCA9685), servo centre, motor direction, button, camera frame.
10. Run on the mat.

---

**Team Astra — WRO Future Engineers 2026 — NEO — A STAR IN MOTION**

Specification lock: 195 × 111 × 122 mm, wheelbase 150 mm, tracks 85 mm, wheel radius 30 mm, mass **700 g**, RWD EV3 Medium + **LEGO Technic differential 1:1**, MP1584 **9.0 V**, steering **60° left / 55° right**, Bonka **12 V** 2200 mAh, Camera 3 Wide on **Stand V3**, **BNO055 top centre / mux 4**, four **VL53L5X** on mux 1/2/3/5, recorded single-lap time **8.7 s**.
