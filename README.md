# DriftCTRL – Deep Reinforcement Learning for CarRacing-v3

This project applies and evaluates major reinforcement learning algorithms (PPO, DQN, A2C) on the CarRacing-v3 environment from OpenAI Gymnasium. It demonstrates performance comparisons between continuous and discretized control strategies, along with training logs, reward analysis, and video recordings of agent behavior.

---

## Project Structure

```
driftCTRL/
├── notebooks/              # Jupyter Notebooks for each agent
│   ├── PPO_driftCTRL.ipynb
│   ├── DQN_driftCTRL.ipynb
│   └── A2C_driftCTRL.ipynb
├── models/                 # Trained model zip files
├── logs/                   # Training monitor logs (CSV)
├── videos/                 # Recorded driving behavior (MP4)
├── requirements.txt        # Required Python packages
└── README.md               # This file
```

---

## Objective

To compare the performance and stability of common reinforcement learning algorithms on a continuous control task using high-dimensional pixel input. The environment used is CarRacing-v3 from the Gymnasium library.

---

## Agent Comparison

| Agent | Action Space | Reward Curve | Behavior | Notes |
|-------|--------------|--------------|----------|-------|
| PPO   | Continuous   | Converges to ~600+ | Smooth and stable driving | Best performer |
| DQN   | Discrete (custom wrapper) | Noisy | Occasionally drifts, unstable | Demonstrates action space adaptation |
| A2C   | Continuous   | Fails to converge | Spins in place | Highlights algorithm limitations in visual control |

---

## Agent Videos

Recorded videos demonstrating trained agent behavior:

- `videos/ppo_driftctrl.mp4`
- `videos/dqn_driftctrl.mp4`
- `videos/a2c_v2_driftctrl.mp4`

---

## Reward Curves

Each notebook includes training reward visualizations. Raw monitor logs are stored in the `logs/` directory and can be reused for evaluation or comparison.

---

## How to Run

1. Install dependencies:

```bash
pip install -r requirements.txt
```

2. Launch JupyterLab:

```bash
jupyter lab
```

3. Run the notebooks:

- PPO: `notebooks/PPO_driftCTRL.ipynb`
- DQN: `notebooks/DQN_driftCTRL.ipynb`
- A2C: `notebooks/A2C_driftCTRL.ipynb`

All required models and video utilities are included.

---

## What I Learned

- PPO is well-suited for continuous control using visual input.
- DQN can be adapted through action discretization, though performance may suffer.
- A2C often fails to converge on pixel-based environments with sparse rewards.
- Utility functions for recording, logging, and saving help make RL projects more reproducible and presentable.


---

## Agent Behavior Summary (Based on Video Evaluation)

### PPO (Proximal Policy Optimization)
- **Behavior:**  
  In the initial rollout, the agent briefly veers off track, but rapidly recovers. In subsequent episodes (within the same video), the agent maintains ~95% track adherence, demonstrating smooth throttle and steering behavior.
- **Interpretation:**  
  This indicates successful convergence and strong policy learning for continuous visual control. Minor excursions onto the grass are corrected quickly, showing the agent has learned a robust driving strategy with generalization across episodes.

### DQN (Deep Q-Network with Custom Discretization)
- **Behavior:**  
  Starts with promising behavior, attempting to stay on track with significant left/right steering. However, due to the coarse discretization of continuous actions, the agent tends to overcorrect, eventually leading it onto the grass. It then transitions into a circular drift/donut pattern, failing to recover.
- **Interpretation:**  
  While the discrete policy shows an attempt to learn lateral control, it lacks fine-tuned correction and stability. The emergent donut behavior is a known pattern in unstable discrete RL setups on continuous tasks.

### A2C (Advantage Actor-Critic)
- **Behavior:**  
  Fails from the beginning. The agent performs immediate rotational motions (spins in place) and makes no attempt to follow the road. Entire video exhibits degenerate repetitive motion.
- **Interpretation:**  
  Indicates non-convergence and poor policy learning. Despite attempts like frame stacking and hyperparameter tuning, A2C could not develop a usable driving strategy. This reflects known limitations of A2C in high-dimensional visual input environments with sparse rewards.

---

## Comparison to Original Project (DQN-only baseline)

| Aspect | Original DQN (car_racer_gym) | Our PPO | Our DQN | Our A2C |
|--------|-------------------------------|----------|----------|----------|
| **Action Space** | Discrete, basic | Continuous | Discrete (custom) | Continuous |
| **Stability** | Unstable drifting | Smooth & stable | Unstable, aggressive steering | Completely failed |
| **Track Adherence** | Low | ~80-90% (after 1st run) | Low | None |
| **Emergent Behavior** | Donuts or crash | Clean recovery | Donuts after veer-off | Immediate donuts |
| **Enhancements** | None | Logging, video, policy saving | Custom wrapper, reward curves | Stack frames, tuned but failed |
| **Overall Result** | Minimal success | Strongest result | Partial attempt | Failed policy |


---

## Acknowledgement

This project was loosely inspired by [AGiannoutsos/car_racer_gym](https://github.com/AGiannoutsos/car_racer_gym), which originally demonstrated DQN on an earlier version of CarRacing. This version expands the idea to include PPO and A2C, modern Gymnasium environments, structured utilities, training logs, and video visualizations.

---

## Author

Ayushman Mishra  
MS in Robotics and Autonomous Systems (Mechanical & Aerospace) – Arizona State University  
GitHub: [frMishR](https://github.com/frMishR)