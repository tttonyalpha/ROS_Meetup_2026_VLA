# ROS Meetup 2026 Vision-Language-Action (VLA) Workshop

<img width="3038" height="1408" alt="prev" src="https://github.com/user-attachments/assets/770911d0-2d3e-4368-9554-f00a130c02aa" />


This repository contains materials for a hands-on ROS Meetup 2026 workshop on **Vision-Language-Action (VLA) models**.

VLA models connect **visual perception**, **language understanding**, and **robot actions** in a single system. In this workshop, we first discuss the core ideas behind VLA models and then move to practical tools for training and deploying them in robotics workflows.

## Workshop Structure

The workshop is divided into two parts:

1. **Part 1 — Introduction to VLA Models**
2. **Part 2 — Practical Robotics with LeRobot and ROS2**

---

## Part 1 — Introduction to VLA Models

In the first part, we cover the fundamentals of VLA models:

- what VLA models are
- how they work
- what data they use
- how they are trained
- common architectural design choices
- a practical example based on **SmolVLA**

### Materials

- [Presentation 1: Introduction to VLA Models]([PASTE_PRESENTATION_1_LINK_HERE](https://docs.google.com/presentation/d/1ulWZXXcL3qXQBPKhKuirijH677_3Eg9J/edit?usp=share_link&ouid=117545374379394505476&rtpof=true&sd=true))
- [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/drive/1cikH79MNhJz5UvSeYsD5kKGzTF8oID3f?usp=sharing)

### Topics covered

- Motivation for VLA models
- From Vision-Language Models to Vision-Language-Action systems
- Action prediction and robot control
- Training data for VLA
- Policy learning and imitation learning
- Overview of popular VLA architectures
- Practical walkthrough with **SmolVLA**

---

## Part 2 — Practical Robotics with LeRobot and ROS2

In the second part, we focus on the practical side of working with VLA models in robotics.

We discuss:

- what kind of robots can work with VLA models
- what hardware requirements such robots typically have
- how to use **LeRobot**
- why **ROS2** can be useful in a LeRobot-based setup

### Materials

- [Presentation 2: LeRobot, Robots, and ROS2](PASTE_PRESENTATION_2_LINK_HERE)
- [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](PASTE_COLAB_2_LINK_HERE)

### Topics covered

#### 1. Robots for VLA
- which robots are compatible with VLA pipelines
- what hardware is needed for deployment
- sensors, cameras, actuators, and compute requirements

#### 2. Working with LeRobot
- how to load a model
- how to collect a dataset
- how to train a model
- how to run inference

The goal of this part is to show that the full workflow in **LeRobot** is accessible and easy to understand.

We also demonstrate:

- how to record an episode
- how the **SO-Arm101** is structured inside LeRobot
- how to extend the framework and add your own robot by analogy

#### 3. Why ROS2 may be needed
- how ROS2 fits into robotics workflows around LeRobot
- when ROS2 becomes useful for communication and integration
- a minimal example using **ROS2 Actions**

---

## Repository Contents

```text
.
├── part1/
│   ├── slides/
│   ├── notebooks/
│   └── assets/
├── part2/
│   ├── slides/
│   ├── notebooks/
│   └── assets/
└── README.md
