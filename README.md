# Unit-7 Survival Chamber

A browser simulation of a self-preserving robot. Unit-7 watches its battery, hull and core temperature. Four times a second it scores five drives (evade, recharge, repair, cool down, explore) and acts on the strongest one, then explains the choice in a decision log.

## Run it

Open `index.html` in a browser. There's no build step and nothing to install.

## Try

- Click the chamber to drop drones, acid pools, heat vents, chargers or repair kits.
- Turn off **Self-preservation** to see how long it lasts without instincts.
- Move the **Caution** slider to change how wide a berth it gives hazards.
- Run the sim at 1×, 2× or 4× speed.

## Learned brain (Q-learning)

Set **Brain** to **Learned** to swap the hand-written rules for a Q-table that starts knowing nothing. It sorts each moment into one of 216 situations (battery, hull, temperature, drone distance, docked) and learns which of the five directives keeps it alive longest. Its reward is +1 per second alive, plus a little for hull and battery gained, and −20 for going offline. **Train 4 hours** runs four simulated hours headless in a few seconds. The Learning card charts every life, and the drive bars show the Q-values for the current situation.

The whole learning rule is one line in `qChoose()`:

```
Q[s][a] += α · (reward + γ · max Q[s', ·] − Q[s][a])
```

On the default chamber the rules brain averages about 12 minutes per life and random choices about 1.5 minutes. The learned brain reaches about 2 minutes after 4 training hours and about 8 minutes after 12. Most of its early deaths come from a flat battery.

Ideas to try from the browser console (`unit7.QL_CFG`, `unit7.QL`):

1. Add "distance to nearest charger" to `qState()` and see whether battery deaths drop.
2. Change `rDeath`, `rBatt` or `gamma` and compare learning curves.
3. Replace the table with a small neural network that takes raw vitals as input. That's a DQN.
