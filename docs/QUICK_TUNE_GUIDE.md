# 🎮 Quick Reward Tuning Guide

## Current Settings (Optimized for Fast Learning)

```python
# In game.py - play_step() method

EATING_FOOD = +10      # When snake eats food
MOVING_CLOSER = +1     # Each step towards food
MOVING_AWAY = -1       # Each step away from food  
DEATH_PENALTY = -10    # Collision with wall/self
```

---

## 🚀 Preset Configurations

### 1. **Aggressive Learning** (Fastest convergence)
```python
# In game.py, line ~90-95
if self.head == self.food:
    reward = 20  # More emphasis on food

if current_dist < prev_dist:
    reward = 3   # Stronger guidance
else:
    reward = -3  # Stronger penalties

if self.is_collision():
    reward = -20  # Much stronger death penalty
```

### 2. **Conservative Learning** (More stable, slower)
```python
if self.head == self.food:
    reward = 15  # Moderate food reward

if current_dist < prev_dist:
    reward = 0.5  # Gentle guidance
else:
    reward = -0.5  # Gentle penalties

if self.is_collision():
    reward = -10  # Standard death penalty
```

### 3. **Exploration Mode** (Try new strategies)
```python
# In agent.py, line ~117
self.epsilon = 120 - self.n_games  # More random moves
```

### 4. **Exploitation Mode** (Use learned behavior)
```python
# In agent.py, line ~117  
self.epsilon = 50 - self.n_games  # Less random moves
```

---

## 📊 If Your AI Is...

### ❌ **Not learning at all (stuck at score 0-1)**
→ Increase distance rewards:
```python
reward = 5 if closer else -5  # Much stronger signals
```

### 🔄 **Too random (won't focus on food)**
→ Decrease epsilon:
```python
self.epsilon = 50 - self.n_games  # Less exploration
```

### 💀 **Dying too much**
→ Increase death penalty:
```python
reward = -20  # Stronger fear of death
```

### 🐌 **Playing too safe (avoiding food)**
→ Increase food reward:
```python
reward = 30  # Make food more attractive
```

### ⚡ **Learning too slow**
→ Use aggressive preset (see above)

---

## 🎯 Testing Strategy

1. **Backup your current model folder** before testing
2. **Change ONE parameter** at a time
3. **Train for 200 games** to see effects
4. **Compare mean scores** in the graph
5. **Keep what works**, revert what doesn't

---

## 📈 Expected Performance Timeline

| Games | Expected Behavior |
|-------|------------------|
| 1-30 | Mostly random, occasional food |
| 30-80 | Starting to seek food, fewer instant deaths |
| 80-150 | Consistently moving towards food |
| 150+ | Optimizing paths, avoiding itself |

**With new reward system, you should see:**
- First food by game ~20-30 (was ~50-80)
- Score 5+ by game ~80 (was ~150)
- Score 10+ by game ~150 (was ~250)

---

## 🔬 Advanced: Dynamic Rewards

Want rewards that adapt during the game?

```python
# In game.py, add after line ~90:

# Rewards get bigger as snake grows (harder to navigate)
difficulty_multiplier = 1 + (len(self.snake) * 0.1)

if current_dist < prev_dist:
    reward = 1 * difficulty_multiplier
else:
    reward = -1 * difficulty_multiplier
```

---

## 💾 Remember!

- Model auto-saves every 10 games
- Best model saved when new record is set  
- Models persist across runs (continuous learning)
- Check `model/metadata.json` for training history

Happy training! 🚀
