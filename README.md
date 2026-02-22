# 🏎️ Unity AI Racing Agent (ML-Agents)

A reinforcement learning project where an autonomous vehicle learns to
drive around a closed racing track using Unity ML-Agents and PPO.

------------------------------------------------------------------------

## 🎯 Project Goal

Train an AI agent that can:

-   Stay on track\
-   Navigate corners reliably\
-   Make consistent forward progress\
-   Improve lap stability over time

Speed optimization is secondary to stability in v1.

------------------------------------------------------------------------

## 🧱 Tech Stack

-   **Engine:** Unity (LTS)
-   **RL Framework:** Unity ML-Agents
-   **Algorithm:** PPO (Proximal Policy Optimization)
-   **Physics:** WheelCollider-based vehicle controller

------------------------------------------------------------------------

## 🏗 Architecture Overview

                    +-----------------------------+
                    |        Python Trainer       |
                    |    (PPO via ML-Agents CLI)  |
                    +-------------+---------------+
                                  |
                                  | Observations
                                  | Rewards
                                  | Actions
                                  v
    +------------------------------------------------------+
    |                  Unity Environment                   |
    |                                                      |
    |   +------------------+       +------------------+    |
    |   |      Agent       | <-->  |   Track + Walls |    |
    |   +------------------+       +------------------+    |
    |                                                      |
    |   - Physics (Rigidbody + WheelColliders)            |
    |   - Waypoint System                                 |
    |   - Raycast Sensors                                 |
    +------------------------------------------------------+

------------------------------------------------------------------------

## 🧠 Agent Design

### Action Space (Continuous)

  Action     Range
  ---------- -----------
  Steering   \[-1, 1\]
  Throttle   \[0, 1\]

Braking is excluded in v1 for stability.

------------------------------------------------------------------------

### Observation Space (\~10 floats)

-   7 raycast distances (±60° spread)
-   Forward speed (normalized)
-   Signed angle to track direction
-   Yaw rate (angular velocity around Y axis)

All values are normalized before being passed to the neural network.

------------------------------------------------------------------------

## 🛣 Environment Design

### Track

-   Closed loop circuit\
-   Waypoint system defines centerline and direction\
-   Drivable surface assigned to `Track` layer\
-   Walls with colliders for collision detection

------------------------------------------------------------------------

## 🏁 Episode Termination

An episode ends when:

-   Collision with wall\
-   Off-track detection\
-   Car stuck below speed threshold\
-   Time limit exceeded

------------------------------------------------------------------------

## 🎁 Reward Structure (v1)

### Positive Reward

-   Proportional to forward progress along track centerline

### Penalties

-   Collision penalty\
-   Off-track penalty\
-   Optional small per-step penalty

Negative progress (reversing) does not generate reward.

------------------------------------------------------------------------

## 📈 Training Strategy

-   PPO via ML-Agents\
-   Conservative hyperparameters for stable learning\
-   Decision period greater than 1 to smooth control\
-   Begin at normal time scale, increase after validation

------------------------------------------------------------------------

## 🚀 Setup Instructions

### 1. Install Dependencies

-   Install Unity LTS\
-   Install ML-Agents via Package Manager\
-   Install Python and ML-Agents CLI

Install ML-Agents Python package:

    pip install mlagents

------------------------------------------------------------------------

### 2. Configure Agent

Add the following components to the car GameObject:

-   **Behavior Parameters**
    -   Behavior Name: `Racer`
    -   Continuous Actions: 2
-   **Decision Requester**
    -   Decision Period: 5
    -   Take Actions Between Decisions: Enabled

------------------------------------------------------------------------

### 3. Train

From project root:

    mlagents-learn config/ppo_racer.yaml --run-id=racer_v1

Press Play in Unity when prompted.

------------------------------------------------------------------------

## 📊 Success Criteria (v1)

The agent can:

-   Drive at least one full lap without crashing\
-   Maintain stable steering behavior\
-   Show increasing cumulative reward over time

------------------------------------------------------------------------

## 🛠 Roadmap

### Phase 1 --- Stability

-   Stay on track\
-   Smooth steering\
-   Complete partial laps

### Phase 2 --- Performance

-   Faster lap times\
-   Improved cornering\
-   Reduced wall proximity

### Phase 3 --- Extensions

-   Add braking\
-   Lap completion bonus\
-   Ghost racing\
-   Domain randomization\
-   Visual-based learning

------------------------------------------------------------------------

## 📂 Suggested Folder Structure

    Assets/
     ├── Scripts/
     │    ├── Agent/
     │    ├── Environment/
     │    ├── Controllers/
     │    └── Utilities/
     ├── Prefabs/
     ├── Scenes/
     └── Config/
          └── ppo_racer.yaml

------------------------------------------------------------------------

## 📎 License

This project is for educational and experimental purposes.
