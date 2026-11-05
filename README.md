# Expressive Robot Arm Motion in MuJoCo

A small simulation project that shows how the **motion style** of a 7-DOF robot arm can express emotion. Emotions are described with the **PAD model** (Pleasure, Arousal, Dominance), and each dimension is mapped to a motion property of the arm. Built with [MuJoCo](https://mujoco.org/) in Google Colab.

> Robots do not need a face to be expressive: speed, size, and posture of movement already carry affective information.

## Demo

Add your rendered videos or GIFs here (see the `media/` folder).

| Condition | PAD (P, A, D) | Expected impression |
|---|---|---|
| Baseline | (0, 0, 0) | Neutral reference |
| Excited / joyful | (1, 1, 1) | Fast, large, lifted movement |
| Sad / depressed | (-1, -1, -1) | Slow, small, drooping movement |
| Fearful / anxious | (-1, 1, -1) | Fast, small, drooping movement |
| Angry / hostile | (-1, 1, 1) | Fast, large, drooping movement |

<!-- ![Baseline](media/baseline.gif) -->
<!-- ![PAD 1,1,1](media/pad_1_1_1.gif) -->

## How the PAD mapping works

Each of P, A, D is in the range [-1, 1] and controls one property of the joint velocities:

| Dimension | Motion property | Implementation |
|---|---|---|
| **Pleasure** | Direction / posture | Bias added to the pitch joints: positive lifts and opens the arm, negative makes it droop |
| **Arousal** | Speed | Oscillation frequency scaled from 0.5x (A = -1) to 2.0x (A = +1) |
| **Dominance** | Expansiveness | Oscillation amplitude scaled from 0.3x (D = -1) to 2.0x (D = +1) |

The pleasure bias is multiplied by an envelope `sin(pi * t / duration)`, so the arm rises and then settles back toward neutral instead of drifting indefinitely.

## The robot

A custom 7-DOF arm defined in MuJoCo XML, with a human-arm-like joint layout:

- **Shoulder (3 DOF):** yaw, pitch, roll
- **Elbow (1 DOF):** pitch
- **Wrist (3 DOF):** roll, pitch, yaw

## Getting started

### Option A: Google Colab

1. Open the notebook in Colab:
   `https://colab.research.google.com/github/<your-username>/Expressive-Robot-Arm-MuJoCo/blob/main/expressive_robot_arm.ipynb`
2. Run the first cell to install dependencies, then run the rest in order.
3. Colab provides the GPU/EGL rendering used for headless video output.

### Option B: Run locally

```bash
git clone https://github.com/<your-username>/Expressive-Robot-Arm-MuJoCo.git
cd Expressive-Robot-Arm-MuJoCo
pip install -r requirements.txt
```

Headless rendering on Linux needs `MUJOCO_GL=egl` (as set in the notebook). On a desktop machine you may use `glfw` instead.

## Repository structure

```
.
├── expressive_robot_arm.ipynb   # main notebook (simulation + PAD experiments)
├── requirements.txt
├── media/                       # demo GIFs / videos
├── LICENSE
└── README.md
```

## Limitations and next steps

- Joint velocities are set directly (`data.qvel`) rather than through actuators or a controller, so this is an expressive motion sketch, not a physically realistic control system.
- The PAD-to-motion mapping is hand-designed. Whether people actually perceive the intended emotions is untested.
- Possible extensions: a user study to evaluate perceived emotion, actuator-based control, additional emotion conditions, or a different robot model.

## Background

Based on the PAD emotional state model (Mehrabian and Russell) and research on affective robot motion. Developed as part of a robotics/simulation course assignment.

## Tech stack

Python, MuJoCo, NumPy, mediapy, Google Colab
