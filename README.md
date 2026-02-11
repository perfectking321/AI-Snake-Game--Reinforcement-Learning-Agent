# 🐍 AI Snake Game - Deep Q-Learning & Hamiltonian Hybrid

<div align="center">

![Python](https://img.shields.io/badge/python-v3.8+-blue.svg)
![PyTorch](https://img.shields.io/badge/PyTorch-ee4c2c?logo=pytorch&logoColor=white)
![License](https://img.shields.io/badge/license-MIT-green.svg)

An intelligent Snake game AI trained using **Deep Q-Learning (DQN)** with an optional **Hamiltonian Cycle safety net** for enhanced performance and guaranteed survival strategies.

[Features](#-features) • [Installation](#-installation) • [Usage](#-usage) • [Architecture](#-architecture) • [Training](#-training) • [Documentation](#-documentation)

</div>

---

## 🎯 Features

- **🧠 Deep Q-Learning Agent**: Neural network-based AI that learns to play Snake through reinforcement learning
- **🔄 Hybrid Agent**: Combines DQN with Hamiltonian cycle algorithm as a safety fallback
- **📊 Real-time Training Visualization**: Live plotting of scores and performance metrics
- **💾 Model Checkpointing**: Automatic saving of best-performing models with metadata
- **🎮 Interactive Demo**: Watch the AI play with visual indicators showing decision-making mode
- **📈 Comprehensive Reward System**: Sophisticated reward shaping for faster learning
- **🧪 Testing Suite**: Multiple test files for validation and comparison

---

## 🚀 Quick Start

### Prerequisites

```bash
Python 3.8+
pip
```

### Installation

1. **Clone the repository**
```bash
git clone https://github.com/yourusername/ai-snake-game.git
cd ai-snake-game
```

2. **Install dependencies**
```bash
pip install -r requirements.txt
```

3. **Run the training**
```bash
# Train standard DQN agent
python agent.py

# Train hybrid agent with Hamiltonian safety net
python agent_hybrid.py
```

4. **Watch a demo**
```bash
python demo_hybrid.py
```

---

## 📖 Usage

### Training a New Agent

**Standard DQN Agent:**
```bash
python agent.py
```
- Trains a pure deep reinforcement learning agent
- Saves best models to `./model/`
- Displays live training metrics and plots

**Hybrid Agent:**
```bash
python agent_hybrid.py
```
- Combines DQN with Hamiltonian cycle fallback
- Switches to Hamiltonian path when danger is detected
- Saves models to `./model_hybrid/`
- Provides statistics on AI vs Hamiltonian decision usage

### Running Demos

```bash
# Visual demonstration of the hybrid agent
python demo_hybrid.py
```
- Green border: AI making decisions
- Yellow border: Hamiltonian cycle active
- Shows real-time score and decision mode

### Testing

```bash
# Run all tests
python tests/integration_test.py
python tests/test_hamiltonian.py
python tests/quick_visual_test.py

# Compare agent performance
python tests/compare_agents.py
```

---

## 🏗️ Architecture

### Project Structure

```
ai-snake-game/
├── 📄 Core Files
│   ├── game.py                 # Snake game environment (Pygame)
│   ├── model.py                # Neural network architecture (PyTorch)
│   ├── agent.py                # Standard DQN agent
│   ├── agent_hybrid.py         # Hybrid DQN + Hamiltonian agent
│   ├── hamiltonian_path.py     # Hamiltonian cycle implementation
│   └── helper.py               # Plotting and utility functions
│
├── 📁 models/
│   ├── model/                  # Standard agent checkpoints
│   └── model_hybrid/           # Hybrid agent checkpoints
│
├── 📁 tests/
│   ├── integration_test.py     # Full system integration tests
│   ├── test_hamiltonian.py     # Hamiltonian path tests
│   ├── quick_visual_test.py    # Visual testing
│   └── compare_agents.py       # Agent performance comparison
│
├── 📁 docs/
│   ├── REWARD_SYSTEM_EXPLAINED.md
│   ├── HAMILTONIAN_INTEGRATION.md
│   ├── COMPLETE_REWARD_SYSTEM.md
│   ├── PERFORMANCE_BONUS_SYSTEM.md
│   ├── QUICK_TUNE_GUIDE.md
│   ├── RECORD_BONUS_SYSTEM.md
│   └── TAIL_REWARD_SYSTEM.md
│
├── 📁 assets/
│   └── arial.ttf               # Font for game display
│
├── 🎮 demo_hybrid.py           # Interactive demonstration
├── 📋 requirements.txt         # Python dependencies
├── 📝 README.md                # This file
└── 🚫 .gitignore               # Git ignore rules
```

### Neural Network Architecture

**Standard Agent (11 inputs → 256 hidden → 3 outputs)**
```python
Input Layer: 11 features
├── Danger detection (3): straight, right, left
├── Direction state (4): up, down, left, right
├── Food location (4): up, down, left, right

Hidden Layer: 256 neurons (ReLU activation)

Output Layer: 3 actions
└── [Straight, Right Turn, Left Turn]
```

**Hybrid Agent (14 inputs → 256 hidden → 3 outputs)**
- Standard 11 features + 3 Hamiltonian features
- Additional inputs for safety assessment

---

## 🧠 How It Works

### Deep Q-Learning (DQN)

The agent uses **Q-Learning** with neural network approximation:

1. **State Perception**: Agent observes game state (11-14 features)
2. **Action Selection**: 
   - Early training: High exploration (random moves)
   - Late training: Exploitation (learned behavior)
3. **Reward Feedback**: Environment provides rewards/penalties
4. **Q-Value Update**: Network learns optimal action-value function
5. **Experience Replay**: Learns from past experiences stored in memory

**Key Equation:**
```
Q(s,a) = r + γ × max Q(s',a')
```
Where:
- `Q(s,a)` = Quality of action `a` in state `s`
- `r` = Immediate reward
- `γ = 0.9` = Discount factor (values future rewards)
- `max Q(s',a')` = Best future Q-value

### Reward System

| Event | Reward | Purpose |
|-------|--------|---------|
| 🍎 Eat Food | +10 | Primary objective |
| ➡️ Move Closer to Food | +1 | Guide towards food |
| ⬅️ Move Away from Food | -1 | Discourage wandering |
| 💀 Die (collision) | -10 | Avoid death |

### Hybrid System

The **Hybrid Agent** combines two strategies:

1. **DQN Mode** (Default): AI makes learned decisions
2. **Hamiltonian Mode** (Safety): Follows guaranteed survival path

**Activation Conditions:**
- Safety score < 30%
- Stuck in dangerous position
- High collision risk detected

**Benefits:**
- Guaranteed survival in critical situations
- Faster learning (safer exploration)
- Higher average scores

---

## 📊 Training

### Training Parameters

```python
MAX_MEMORY = 100,000      # Experience replay buffer size
BATCH_SIZE = 1,000        # Training batch size
LEARNING_RATE = 0.001     # Neural network learning rate
GAMMA = 0.9               # Discount factor for future rewards
EPSILON = 80 - n_games    # Exploration rate (decreases over time)
```

### Training Progress

The agent typically shows improvement in stages:

| Games | Avg Score | Behavior |
|-------|-----------|----------|
| 0-50 | 0-5 | Random exploration, frequent deaths |
| 50-100 | 5-15 | Learning food direction |
| 100-200 | 15-30 | Avoiding walls, basic strategy |
| 200+ | 30-60+ | Advanced patterns, survival |

Best recorded scores: **60+** (Standard), **Higher with Hybrid**

### Monitoring Training

- **Live Plot**: Real-time score visualization
- **Console Output**: Game number, score, record, mean score
- **Model Checkpoints**: Saved after each new record
- **Metadata**: Training statistics saved with models

---

## 🔧 Configuration

### Adjusting Game Speed

In `game.py`:
```python
SPEED = 5000  # Higher = slower gameplay
```

### Modifying Reward Values

In `game.py`, locate the reward calculation:
```python
reward = 0
if game_over:
    reward = -10
    return reward, game_over, self.score

if self.score > score:
    reward = 10
```

### Tuning Learning Parameters

See [docs/QUICK_TUNE_GUIDE.md](docs/QUICK_TUNE_GUIDE.md) for detailed parameter tuning.

---

## 📚 Documentation

Comprehensive documentation available in `/docs`:

- **[REWARD_SYSTEM_EXPLAINED.md](docs/REWARD_SYSTEM_EXPLAINED.md)** - Complete reward system breakdown
- **[HAMILTONIAN_INTEGRATION.md](docs/HAMILTONIAN_INTEGRATION.md)** - Hybrid system architecture
- **[QUICK_TUNE_GUIDE.md](docs/QUICK_TUNE_GUIDE.md)** - Parameter tuning guide
- **[COMPLETE_REWARD_SYSTEM.md](docs/COMPLETE_REWARD_SYSTEM.md)** - Advanced reward strategies
- **[PERFORMANCE_BONUS_SYSTEM.md](docs/PERFORMANCE_BONUS_SYSTEM.md)** - Performance optimization
- **[RECORD_BONUS_SYSTEM.md](docs/RECORD_BONUS_SYSTEM.md)** - Record tracking system
- **[TAIL_REWARD_SYSTEM.md](docs/TAIL_REWARD_SYSTEM.md)** - Tail avoidance strategies

---

## 🎮 Controls & Visualization

### Game Display

- **Blue Segments**: Snake body
- **Red Square**: Food
- **White Background**: Play area
- **Score Display**: Top of window

### Demo Mode Indicators

- **Green Border**: AI in control (DQN)
- **Yellow Border**: Hamiltonian safety mode active

---

## 🧪 Testing

### Available Tests

```bash
# Integration test - full system check
python tests/integration_test.py

# Hamiltonian path validation
python tests/test_hamiltonian.py

# Quick visual verification
python tests/quick_visual_test.py

# Compare standard vs hybrid agents
python tests/compare_agents.py
```

---

## 📈 Performance

### Benchmarks

**Standard DQN Agent:**
- Average Score (200+ games): 25-35
- Best Score: 60+
- Training Time: ~30-60 minutes to convergence

**Hybrid Agent:**
- Average Score (200+ games): 30-45
- Best Score: 65+
- Training Time: ~45-75 minutes to convergence
- Hamiltonian Usage: 10-20% of decisions

*Benchmarks based on default parameters on consumer hardware*

---

## 🛠️ Technologies Used

- **[Python 3.8+](https://www.python.org/)** - Core language
- **[PyTorch](https://pytorch.org/)** - Deep learning framework
- **[Pygame](https://www.pygame.org/)** - Game engine and visualization
- **[NumPy](https://numpy.org/)** - Numerical computations
- **[Matplotlib](https://matplotlib.org/)** - Training visualization

---

## 🤝 Contributing

Contributions are welcome! Here's how you can help:

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/AmazingFeature`)
3. Commit your changes (`git commit -m 'Add some AmazingFeature'`)
4. Push to the branch (`git push origin feature/AmazingFeature`)
5. Open a Pull Request

### Areas for Contribution

- 🎯 New reward strategies
- 🧠 Alternative neural network architectures
- 📊 Enhanced visualization
- 🧪 Additional test cases
- 📚 Documentation improvements
- 🎮 UI/UX enhancements

---

## 📝 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

---

## 🙏 Acknowledgments

- Inspired by classic Snake game implementations
- Deep Q-Learning algorithm from DeepMind's DQN paper
- Hamiltonian cycle approach for guaranteed survival
- Pygame community for excellent documentation

---

## 📧 Contact

For questions, suggestions, or issues:

- **GitHub Issues**: [Create an issue](https://github.com/yourusername/ai-snake-game/issues)
- **Email**: your.email@example.com

---

## 🔮 Future Enhancements

- [ ] Multi-agent training environment
- [ ] Convolutional Neural Network (CNN) for visual input
- [ ] Dueling DQN architecture
- [ ] Prioritized experience replay
- [ ] Web-based demo interface
- [ ] Tournament mode for agent comparison
- [ ] Save/load training sessions
- [ ] Configurable game board sizes

---

<div align="center">

**⭐ If you find this project useful, please consider giving it a star! ⭐**

Made with ❤️ and 🐍

</div>
