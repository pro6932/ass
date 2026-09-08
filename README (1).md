# TEAM ASTRA — WRO FUTURE ENGINEERS 2026

<p align="center">
  <img width="800" alt="Team Astra / NEO" src="https://github.com/user-attachments/assets/e0de9b6c-283e-4a41-941f-7965bcdf1862" />
</p>

<p align="center"><b>A STAR IN MOTION</b></p>

Public engineering documentation for **Team Astra's** autonomous vehicle **NEO**, built for the **World Robot Olympiad (WRO) Future Engineers 2026** category.

This README is the entry point to the repository. It states the current vehicle specification, explains the mobility / power / sensing / obstacle architecture, maps software modules to electromechanical hardware, and describes how another team can rebuild and run the system.

A separate Engineering Journal PDF (print copy for competition day) should be generated from this repository before the documentation deadline.

---

## Rule compliance (vehicle regulations)

NEO is designed inside the official Future Engineers vehicle limits:

| Rule | Limit | NEO (measured / current) |
|---|---|---|
| Length × width × height | ≤ 300 × 200 × 300 mm | **195 × 111 × 122 mm** |
| Mass | ≤ 1.5 kg | **700 g (0.70 kg)** |
| Wheels | 4 wheels | 4 wheels, 30 mm radius |
| Drive | one driving axle | **rear axle, rear-wheel drive** |
| Steering | one steering actuator | **SG90 micro servo**, front wheels |
| Control | fully autonomous | Raspberry Pi 5; no remote driving |

Dimensions and mass will be re-checked at robot inspection. The vehicle outline is not intended to change during a round.

---

## Team Astra

Three-member team. Mentor does not build or program the vehicle.

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

Place official and informal team photos in `t-photos/` before submission.

---

## Current build status (honest configuration)

This section exists so judges are not given a finished-system story that the hardware does not match.

| Item | Status on this revision |
|---|---|
| Chassis, drivetrain, differential, wheels | Installed |
| SG90 steering | Installed; angles estimated by eye |
| Raspberry Pi 5, motor driver, PWM board, I²C mux, buck converters | Installed |
| Bonka 12 V LiPo | Installed |
| Time-of-Flight sensors (rear, front-left, front-centre, front-right) | Installed at the positions below |
| Raspberry Pi Camera 3 Wide | **Not attached yet.** Mount parts exist. Vision text below is the intended pipeline, not a claim that the camera is on the robot today. |
| BNO055 / any gyro / IMU | **Not installed.** Earlier software experiments are kept only as history. |
| Open / Obstacle competition videos | Draft links present; replace with final ≥30 s autonomous clips |

When the camera is fitted, this README will be updated with mount height, tilt angle, and a photo of the installed camera.

---

## NEO at a glance

| Specification | Value |
|---|---|
| Length | 195 mm (19.5 cm) |
| Width | 111 mm (11.1 cm) |
| Height | 122 mm (12.2 cm) |
| Wheelbase | 150 mm (15.0 cm) |
| Front track width | 85 mm (8.5 cm) |
| Rear track width | 85 mm (8.5 cm) |
| Wheel radius | 30 mm (3.0 cm) |
| Wheel diameter | 60 mm |
| Mass | **700 g** |
| Drive | Rear-wheel drive, one driven axle |
| Drive motor | LEGO EV3 Medium Motor |
| Rear axle | LEGO Technic differential |
| Steering | SG90 micro servo, front-wheel steering |
| Observed steering (visual estimate, no protractor) | Left ≈ 60°. Right a little above 45° |
| Main controller | Raspberry Pi 5 |
| Intended camera | Raspberry Pi Camera 3 Wide (**not fitted**) |
| Distance sensors | Four ToF sensors (see placement table) |
| Battery | **Bonka 12 V LiPo** (2200 mAh pack used in the power section) |
| Construction | LEGO Technic + custom 3D-printed PLA |
| Printer | Bambu Lab A1 |
| Observed single-lap time on the mat | **8.7 s** (one round of the mat, not three laps) |

<p align="center">
  <img width="520" alt="NEO" src="https://github.com/user-attachments/assets/5bdba160-a6bc-4c59-a5e1-105b41ca154e" />
</p>

---

## Vehicle photographs (required)

Official requirement: vehicle from every side, top and bottom.

| Front | Rear | Left |
|:---:|:---:|:---:|
| <img src="https://github.com/user-attachments/assets/08a80e9d-fb54-4c7f-a078-1f7dcefe13ed" width="280" alt="Front"> | <img src="https://github.com/user-attachments/assets/c562da21-df0f-404e-8824-0cfd6bfd905d" width="280" alt="Rear"> | <img src="https://github.com/user-attachments/assets/da662955-3f9c-422a-923d-9d184cdc3dea" width="280" alt="Left"> |

| Right | Top | Bottom |
|:---:|:---:|:---:|
| <img src="https://github.com/user-attachments/assets/963cc6fa-45cb-4701-9f0c-07ff7d0bee48" width="280" alt="Right"> | <img src="https://github.com/user-attachments/assets/0baa967b-d792-4137-b402-6e7e74f4cb5b" width="280" alt="Top"> | <img src="https://github.com/user-attachments/assets/d8f873e9-2b76-4520-8104-242555b0fc41" width="280" alt="Bottom"> |

Also keep copies in `v-photos/` as `front.jpg`, `rear.jpg`, `left.jpg`, `right.jpg`, `top.jpg`, `bottom.jpg`.

---

## Performance videos (required)

Official requirement: one YouTube video per challenge; the autonomous driving portion must be **at least 30 seconds**.

| Challenge | Link | Notes |
|---|---|---|
| Open Challenge | https://youtu.be/b3JmTygBYIU | Replace with the final clip if this is only a development run |
| Obstacle Challenge | https://youtu.be/H_eWYqw8Qmo | Replace with the final clip if this is only a development run |

Store the same URLs in `video/video.md`. Do not describe a video as “final” until the autonomous segment is ≥30 s and matches the current hardware (no camera in frame if the camera is still off the robot).

---

# 1. Mobility and mechanical design

NEO is a compact rear-wheel-drive vehicle. Propulsion and steering are separate:

- **Rear axle:** LEGO EV3 Medium Motor → LEGO differential → rear wheels
- **Front axle:** SG90 servo → steering linkage → front wheels (not driven)

That split keeps the steering mechanism independent of drivetrain torque and matches the rule “one driving axle and one steering actuator”.

## 1.1 Geometry

| Parameter | mm | cm |
|---|---:|---:|
| Length | 195 | 19.5 |
| Width | 111 | 11.1 |
| Height | 122 | 12.2 |
| Wheelbase | 150 | 15.0 |
| Front track | 85 | 8.5 |
| Rear track | 85 | 8.5 |
| Wheel radius | 30 | 3.0 |
| Mass | 700 g | 0.70 kg |

Why these numbers:

- **150 mm wheelbase** leaves room for the battery, Pi, converters and rear differential without growing past 300 mm length.
- **85 mm tracks** keep the vehicle narrow on the inner-wall / pillar field while still giving a stable 111 mm overall width.
- **60 mm wheels** give ground clearance for the ToF sensors (mounted 65–70 mm high) without pushing height near 300 mm.
- **700 g** is well under the 1.5 kg limit, which reduces load on the EV3 Medium Motor and on the small SG90.

## 1.2 Drive system

```
EV3 Medium Motor  →  LEGO differential  →  rear axle  →  rear wheels
```

The EV3 Medium Motor was chosen because it is compact enough for this chassis and meshes directly with Technic gears, axles and the differential. A custom gearbox was not required.

The differential is required because the outside rear wheel travels farther than the inside wheel in a turn. Without it, one wheel would scrub, which wastes motor torque and makes steering response less repeatable.

Observed performance: **one round of the official-size mat in 8.7 s**. This is a measured lap time, not a calculated top speed. Maximum motor RPM on the vehicle has not been measured with a tachometer.

## 1.3 Wheel and speed calculations

Wheel circumference:

\[
C = 2\pi r = 2\pi(0.030) \approx 0.1885\ \text{m}
\]

One wheel revolution theoretically moves NEO **0.1885 m** if there is no slip.

For documentation only, three laps of the mat were estimated at **26.4 m** total path length (about 8.8 m per lap). That estimate is **not** the same measurement as the 8.7 s single lap.

| Case | Distance | Time | Mean speed | Wheel speed if 1:1 and no slip |
|---|---:|---:|---:|---:|
| Observed single lap | ≈ 8.8 m (from the 26.4 m / 3 estimate) | 8.7 s | ≈ 1.01 m/s | ≈ 322 RPM |
| Theoretical 3 laps @ 100% (planning figure) | 26.4 m | 27 s | ≈ 0.98 m/s | ≈ 311 RPM |
| Theoretical 3 laps @ 80% (planning figure) | 26.4 m | 32 s | ≈ 0.83 m/s | ≈ 262 RPM |

Assumptions that still need a test log:

- effective gear ratio treated as 1:1 until teeth are counted and written down
- no wheel slip, no differential loss
- 26.4 m is a geometric estimate, not a measuring-wheel survey

These figures must stay separate from the **8.7 s** stopwatch result.

## 1.4 Steering

Front-wheel steering is driven by an SG90 through a 3D-printed stand and a 13 mm servo horn.

No protractor was available at the time of writing. By eye:

| Direction | Approximate mechanical maximum |
|---|---|
| Left | ≈ 60° |
| Right | a little above 45° |

The left/right range is **not symmetric**. Software must therefore use separate left and right limits rather than one ±angle. Exact servo pulse widths for centre / left stop / right stop will be written here after a protractor or paper-template measurement.

Until that calibration exists, autonomous commands must stay inside conservative software limits so the horn is not driven into the chassis.

## 1.5 Chassis and printed parts

Hybrid construction:

- **LEGO Technic** — axles, gears, differential, structural beams, wheels
- **Printed PLA (Bambu Lab A1)** — parts that LEGO does not provide at the right geometry

Printed parts:

| Part | Function | On the robot now? |
|---|---|---|
| FE2026 custom chassis | Interface between Technic drivetrain and electronics | Yes |
| Servo stand | Holds the SG90 | Yes |
| Servo horn 13 mm | Servo to steering linkage | Yes |
| Pi Camera 3 mount | Holds Camera 3 Wide | Printed; **camera not fitted** |
| Camera stand V2 | Earlier camera tower | Superseded |
| Camera stand V3 | Current camera tower | Ready; waiting for camera install |

STL / CAD files belong in `models/`.

---

# 2. Power and sensor architecture

## 2.1 Electrical architecture

```
ToF sensors (+ intended camera)
        ↓
 Raspberry Pi 5
        ↓
 navigation decision
        ↓
 PCA9685 → SG90          TB6612FNG → EV3 Medium Motor → differential → rear wheels
```

Power is split so motor current does not collapse the Pi rail:

```
Bonka 12 V LiPo
   ├─ XL4015 buck (5 V / 5 A class) → Raspberry Pi 5
   └─ MP1584 buck (3 A class)       → motor-side electronics (driver / servo rail as wired)
```

<p align="center">
  <img width="720" alt="Hardware layout" src="https://github.com/user-attachments/assets/684f0397-d396-4e48-9a4c-e5d33502d106" />
</p>

A full connection diagram belongs in `schemes/` as PDF or PNG. Keep a pin table next to it (I²C mux channels, PCA9685 servo channel, TB6612FNG PWM / IN pins, button GPIO).

## 2.2 Battery and energy

| Item | Value |
|---|---|
| Pack | Bonka 12 V LiPo |
| Capacity used in this document | 2200 mAh = 2.2 Ah |
| Nominal stored energy | \(12 \times 2.2 \approx 26.4\) Wh |

A 3-cell pack is often labelled 11.1 V nominal and ~12.6 V fresh off the charger. This repository uses the team’s working name **12 V Bonka**. Runtime is **not** 26.4 Wh divided by a guessed wattage; it depends on Pi load, motor duty, converter efficiency and cutoff voltage. Measure minutes-to-undervoltage before the event and put the number here.

Charge only with the **iMAX B6AC** balance charger. Confirm pack voltage before every run. The pack is fixed in the chassis so it cannot slide during acceleration or turns.

## 2.3 Why Raspberry Pi 5

OpenCV colour detection and later camera-based pillar logic need more compute than a small microcontroller. The Pi 5 gives CSI camera support, I²C, GPIO and Python on one board. Cost: higher current than a microcontroller, which is why the Pi has its own 5 V buck and is isolated from the motor converter.

## 2.4 Distance sensors (installed)

Four Time-of-Flight sensors. Positions below are the current build, measured from the **rear edge** of the vehicle and from the ground.

| Sensor | Height from ground | Longitudinal position | Lateral |
|---|---|---|---|
| Rear | 65 mm (6.5 cm) | 45 mm (4.5 cm) from the rear edge | rear facing |
| Front-left | 70 mm (7.0 cm) | 170 mm (17 cm) from the rear edge | left front |
| Front-centre | 68 mm (6.8 cm) | 190 mm (19 cm) from the rear edge | vehicle centreline (width / 2) |
| Front-right | 70 mm (7.0 cm) | 170 mm (17 cm) from the rear edge | right front |

Heights sit near the expected wall / pillar band and above the floor so the beams are less likely to read the mat. The centre sensor looks along the vehicle axis. Left and right sensors watch the front corners. The rear sensor is for reverse / parking clearance.

Several ToF devices share I²C addresses, so they are switched through a **TCA9548A** mux. Channel numbers must be listed in `hardware/sensors.md` and in code comments.

## 2.5 Camera (not attached)

Intended sensor: **Raspberry Pi Camera 3 Wide** on CSI.

It is **not on the robot in this revision**. The printed stand and mount are ready. Until it is fitted:

- Open Challenge navigation must be written so it can run on **ToF only**
- Obstacle colour logic is specified below as the **target** pipeline, not as a live sensor

After install, record: mount height, tilt, distance from front bumper, a photo, and whether barrel distortion at the image edges affects pillar colour.

Target capture settings (software side, for when the camera exists):

| Parameter | Target |
|---|---|
| Resolution | 640 × 480 |
| Frame rate | up to ~30 FPS |
| Library | OpenCV + Python |

## 2.6 Actuator electronics

| Board | Role |
|---|---|
| PCA9685 | Hardware PWM for the SG90 so the Pi does not bit-bang servo timing |
| TB6612FNG | Direction and speed for the EV3 Medium Motor; Pi GPIO cannot supply motor current |
| Push button | Physical start / stop input during tests and rounds |

## 2.7 Power budget (to be filled with meter readings)

The rubric asks for current reasoning, not only a battery name. Replace the placeholders after a USB meter / clamp measurement.

| Load | Rail | Measured current | Notes |
|---|---|---|---|
| Raspberry Pi 5 idle | 5 V (XL4015) | _measure_ | |
| Pi + ToF + mux + PCA9685 | 5 V | _measure_ | |
| Pi + intended camera | 5 V | _measure after camera install_ | |
| EV3 motor, vehicle on stand | motor rail | _measure_ | |
| EV3 motor, pushing on mat | motor rail | _measure_ | |
| SG90 holding / moving | servo rail | _measure_ | |

Failure points already designed against:

- motor current sagging the Pi → separate bucks
- loose LiPo in a crash → strapped in the chassis
- wiring in gears → routed away from the differential and steering
- servo stall at the mechanical stop → software limits (numbers still to be measured)

No gyro is in the power or I²C tree.

---

# 3. Software architecture and obstacle strategy

Primary language: **Python** on the Raspberry Pi 5.

Official README requirement: name the modules, say which hardware they drive, and say how to run them.

## 3.1 Module map (code ↔ hardware)

| Software module (target layout under `src/`) | Talks to | Responsibility |
|---|---|---|
| `sensors_tof.py` | TCA9548A + ToF sensors | Read rear / FL / FC / FR distances in mm |
| `vision.py` | Pi Camera 3 Wide on CSI | **Disabled until the camera is attached.** HSV masks, contours, obstacle colour and image-x position |
| `steering.py` | PCA9685 → SG90 | Centre, left, right, proportional angle, software limits |
| `motor.py` | TB6612FNG → EV3 Medium Motor | Forward / reverse / stop / duty |
| `button.py` | GPIO push button | Start wait and emergency stop |
| `fsm_open.py` | ToF + steering + motor | Open Challenge: follow walls / complete laps with random inner walls |
| `fsm_obstacle.py` | ToF + (later vision) + steering + motor | Obstacle Challenge: colour side-pass + parking |
| `config.py` | none | HSV ranges, distances, Kp, servo pulses, motor duty — all tunable constants |
| `main_open.py` / `main_obstacle.py` | all of the above | Challenge entry points |

Until the camera is fitted, `vision.py` must not be required to start `main_open.py`.

## 3.2 Control loop

```
START
  read ToF (and camera frame if attached)
  classify situation
  select FSM state
  compute steering
  compute motor duty
  write PCA9685 + TB6612FNG
REPEAT
```

Priority (highest first):

1. Immediate collision response (ToF below emergency threshold)
2. Obstacle / pillar response (when vision is live)
3. Navigation correction (wall distance / heading proxy from side sensors)
4. Drive straight

## 3.3 Finite-state machine

| State | Use |
|---|---|
| `DRIVE_STRAIGHT` | Path ahead clear enough |
| `STEER_PROPORTIONAL` | Correct using error × Kp |
| `EMERGENCY_DODGE` | Front clearance too small |
| `STOP` | Button, finish, or unsafe reading |
| `PARK` (Obstacle, in progress) | Use rear + side ToF to enter the parking box |

Proportional steering:

\[
\text{error} = \text{desired} - \text{measured}
\]
\[
\text{steering} = K_p \times \text{error}
\]

then clamp to the (still estimated) left/right limits. `Kp`, centre pulse and distance setpoints live in `config.py`, not hard-coded in the FSM.

## 3.4 Open Challenge strategy (ToF-first)

Open Challenge: three laps, inner walls randomised, driving direction randomised after inspection.

Current sensing that actually exists:

- front-centre distance → slow / dodge / stop
- front-left vs front-right → which side is opening
- rear distance → not used for normal forward laps

Direction after the start can be inferred from which side sensor stays closer to a wall over the first section. That logic must be tested on both clockwise and counter-clockwise setups; write the result into the journal when you have it.

## 3.5 Obstacle Challenge strategy (vision planned, ToF live)

Intended visual rule (standard FE pillar logic; confirm against the current season wording before the event):

- detect **red** and **green** regions in HSV
- pass on the side required by that colour
- do not move pillars
- after the three scoring laps, park in the lot; parking may be opposite to the race direction
- a park counts when the plan view is inside the box and the vehicle is parallel (wheel-to-wall difference ≤ 2 cm)

Vision pipeline **when the camera is attached**:

```
capture 640×480
  → BGR to HSV
  → red mask (including hue wrap-around) and green mask
  → denoise / reject tiny contours
  → bounding box, centre x = x + w/2
  → colour + image position + ToF clearances
  → FSM
```

Publish the actual HSV tuples in `config.py`. Do not leave “red / green” as words only.

Parking support that already has hardware: rear ToF at 65 mm height, 45 mm from the rear edge, plus the two front-corner sensors.

## 3.6 How to build, load and run the code

This is the procedure judges are told to look for in the README.

### Raspberry Pi image

1. Flash the current Raspberry Pi OS recommended for Pi 5.
2. Enable I²C and the CSI camera interface (`raspi-config` or `/boot/firmware/config.txt`).
3. Connect the Camera 3 Wide **only when the mount is installed**; leave the ribbon unseated until then.
4. Install system packages and Python libraries used by this repo (OpenCV, SMBus/I²C, PCA9685 library, any VL53 driver you actually import).
5. Copy `src/` onto the Pi (git clone of this repository is preferred).
6. Confirm devices:

```bash
sudo i2cdetect -y 1
# expect TCA9548A and, on each mux channel, the ToF address
```

7. Confirm servo centre with a test script before any autonomous run.
8. Confirm motor direction on a stand so the vehicle cannot drive off the table.
9. Start sequence: power pack → converters → Pi boot → wait for button → `python3 src/main_open.py` or `python3 src/main_obstacle.py`.

Exact package names and a `requirements.txt` must live in `software/setup.md`. Keep that file in sync with the Pi.

### Comments and reproducibility

Every public function that talks to hardware should state the pin, mux channel or PCA9685 channel in a comment. Judges may not have your laptop IDE.

---

# 4. Systems thinking and engineering decisions

## 4.1 Constraints we designed against

- 300 × 200 × 300 mm envelope → finished size 195 × 111 × 122 mm
- 1.5 kg cap → built mass **700 g**
- four wheels, one driven axle, one steering actuator → RWD + front servo
- random inner walls and random direction → cannot hard-code a single path
- documentation must let another team reproduce the car → CAD, wiring, code, this README

## 4.2 Trade-offs

| Decision | Why | Cost |
|---|---|---|
| Raspberry Pi 5 | Enough compute for later OpenCV | Higher current; needs its own 5 V buck |
| Camera 3 Wide (planned) | Wide field for pillars and corners | Distortion at edges; **not fitted yet** |
| Four ToF + mux | Numeric clearance from four directions | Wiring and channel management |
| Rear-wheel drive | Steering stays mechanically simple | Needs rear traction; 700 g helps |
| LEGO differential | Different rear wheel speeds in turns | Extra backlash and parts |
| SG90 | Small and light | Low torque; asymmetric 60° / ~45° range |
| LEGO + printed PLA | Fast mechanical changes + custom mounts | Two construction systems to align |
| Two buck converters | Pi rail isolated from motor noise | Extra modules and wires |
| No gyro on this revision | ToF (+ later camera) is the navigation plan; IMU not required to move | No independent heading angle |
| Fixed HSV ranges (planned) | Cheap on CPU | Sensitive to lighting |

## 4.3 Iterations that already happened

- Camera stand **V2 → V3** (geometry refined; camera still not mounted)
- IMU / BNO055 software was tried, then **removed from the physical robot**. The code can stay in a `legacy/` folder so the decision is visible. It will only return if a test shows heading data beats ToF + camera.
- Dual-converter power architecture added so motor stalls would not reboot the Pi.

Further iterations that must be logged with a date and a result (even a failure):

- measure real steering angles
- count differential / motor gear teeth (replace the 1:1 assumption)
- camera install and first HSV calibration under hall lighting
- parking box trials with the rear ToF

## 4.4 Risks

| Risk | Effect | Mitigation |
|---|---|---|
| Camera still off the robot | No colour for pillars | Keep a ToF-only Open path; do not claim Obstacle colour works until the camera is on |
| Steering range asymmetric | Left and right turns are not equal | Separate software limits; measure with a protractor |
| SG90 stall | Horn or servo dies | Software limits; check linkage free motion |
| LiPo sag / disconnect | Pi brownout | Separate bucks; strap the pack; pre-run voltage check |
| I²C address clash | Dead ToF | TCA9548A channels documented |
| Wide-angle colour errors | Wrong pillar side | Calibrate HSV on the real mat after camera install |
| Unmeasured motor current | Surprise cutoff mid-round | Fill the power-budget table |

---

# 5. Reproducibility and repository layout

Use the official Future Engineers layout so judges can find files quickly:

```
t-photos/          team photographs
v-photos/          front rear left right top bottom
video/video.md     YouTube URLs
schemes/           wiring PDF / PNG and pin table
src/               Python modules listed in §3.1
models/            STL / CAD for printed parts
other/             BOM, calculations, setup notes
docs/              Engineering Journal PDF + extra markdown
README.md          this file
```

Official template: https://github.com/World-Robot-Olympiad-Association/wro2022-fe-template

Git rules from the 2026 documentation section:

- repository **public**
- README in English and **≥ 5000 characters** (this file)
- at least **three commits** on the timetable: 2 months / 1 month / 2 weeks before the competition
- first of those commits already contains **≥ 1/5** of the final code
- the 2-week commit is the snapshot judges may freeze
- stay public at least 12 months after the event

Commit messages should say what changed and what was tested, not “update”.

---

# Parts list (current)

| Component | Role | On robot now |
|---|---|---|
| Raspberry Pi 5 | Controller | Yes |
| Raspberry Pi Camera 3 Wide | Vision | **No** |
| LEGO EV3 Medium Motor | Drive | Yes |
| LEGO differential | Rear axle | Yes |
| SG90 micro servo | Steering | Yes |
| TB6612FNG | Motor driver | Yes |
| PCA9685 | Servo PWM | Yes |
| TCA9548A | I²C mux | Yes |
| ToF sensors (incl. VL53-class devices used on the car) | Distance | Yes (four positions) |
| Bonka 12 V LiPo | Energy | Yes |
| XL4015 5 V buck | Pi rail | Yes |
| MP1584 buck | Motor-side rail | Yes |
| Push button | Operator input | Yes |
| iMAX B6AC | Balance charger (off-robot) | Pit equipment |
| BNO055 | IMU | **No** |
| LEGO Technic + printed PLA | Structure | Yes |

---

# Assembly sequence

1. Print the six PLA parts (camera mount can be printed now even though the camera is off).
2. Build the Technic chassis to 195 × 111 × 122 mm with 150 mm wheelbase and 85 mm tracks.
3. Fit EV3 motor, differential and rear wheels; confirm both rear wheels can rotate at different speeds by hand.
4. Fit servo stand, SG90, 13 mm horn and front steering; confirm the linkage does not hit the chassis.
5. Mount Pi, PCA9685, TCA9548A, TB6612FNG, XL4015, MP1584, button.
6. Fit and strap the Bonka 12 V pack. Wire: pack → XL4015 → Pi; pack → MP1584 → motor electronics. Check polarity before applying power.
7. Mount ToF sensors at the heights and setbacks in §2.4.
8. **Do not** dress the CSI ribbon as if the camera were present. Install the camera later, then lock stand V3 and record the pose.
9. Bench-test I²C, servo centre, motor direction, button.
10. Only then run on the mat.

---

# Possible improvements (next work, in order)

1. Attach the Camera 3 Wide. Record height, angle, photo, and first HSV ranges under competition-like light.
2. Measure steering with a protractor or printed degree template. Replace “≈60° / a little above 45°”.
3. Measure turning radius on paper.
4. Fill the current-draw table; estimate runtime.
5. Count gear teeth; drop the 1:1 assumption if it is wrong.
6. Log Open Challenge runs in both directions with random inner walls.
7. Implement and test parking against the 2 cm parallel rule.
8. Add a one-page dated decision log (what broke, what you changed, commit hash).
9. Export `docs/engineering-journal.pdf` for the hard-copy submission.

---

# Project file index

| Topic | Path to keep in the repo |
|---|---|
| Mobility calculations | `other/calculations.md` or `mobility/calculations.md` |
| Drivetrain | `mobility/drivetrain.md` |
| Steering | `mobility/steering.md` |
| Wiring | `schemes/` |
| Sensors | `hardware/sensors.md` |
| Vision notes | `software/vision.md` |
| Control / FSM | `software/control.md` |
| Pi setup | `software/setup.md` |
| Source | `src/` |
| CAD / STL | `models/` |
| Photos | `t-photos/`, `v-photos/` |
| Videos | `video/video.md` |

---

**Team Astra — WRO Future Engineers 2026 — NEO — A STAR IN MOTION**

Last specification lock in this file: length 195 mm, width 111 mm, height 122 mm, wheelbase 150 mm, tracks 85 mm, wheel radius 30 mm, mass **700 g**, RWD EV3 Medium + differential, steering visual estimate 60° left / >45° right, Bonka **12 V** LiPo, four ToF sensors at the positions in §2.4, **camera not attached**, **no gyro**, observed single-lap time **8.7 s**.
