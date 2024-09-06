# Prisoner's Dilemma

**Authors**: Mohammad Doleh and Yazeed Karajih @karajih

Prisoner's Dilemma is one of the most famous dilemmas where two prisoners are taken into separate rooms, unable to communicate, and each one is given the choice to either stay silent or betray the other prisoner. If both prisoners choose to stay silent, each will go to prison for one year. If one betrays and the other stays silent, the betrayer goes free, and the other prisoner goes to jail for 3 years. If both betray each other, both will go to jail for two years.

## Dilemma Summary

| Our Agent | Opponent | Reward |
|-----------|----------|--------|
| Cooperate | Cooperate | 3      |
| Cooperate | Defect    | 0      |
| Defect    | Cooperate | 5      |
| Defect    | Defect    | 1      |

This dilemma has real-life applications in politics and business. We became curious about how a reinforcement learning (RL) agent would respond and what it considers the best decision.

We opted for Q-Deep learning, as a Q-Table was not feasible due to 2^10 states, and we were keen to explore neural networks.

## Strategies

Our agent learns against these strategies, derived from Axelrod’s Tournament (1980):

- **Tit for Tat**: Cooperates first, then copies the opponent’s last move.
- **Cooperate**: Always cooperate.
- **Random**: Random actions.
- **Tit for Two Tat**: Cooperates unless the opponent defected twice in a row.
- **Grudger**: Cooperates until the opponent defects, then always defects.
- **Pavlov**: Win-stay, lose-switch.
- **Soft Grudger**: Cooperates until defect, defects four times, then returns to cooperation.
- **Fortress**: Defects unless the opponent's last two moves were to cooperate.
- **Sneaky**: Cooperates, then copies the opponent's last move if they cooperated, with a 10% chance of defecting.

## Environment Setup

The state is an array of the last 10 moves, with two possible actions: defect or cooperate (0 or 1). Each game consists of 200 rounds, and the goal is to maximize the win rate by making the opponent spend more years in prison.

## Results

We initially tested our agent with random actions, achieving an average win rate of 0.4 against our strategies. After training with Deep Q-Networks (DQN) using a BoltzmannQPolicy, our agent achieved a win rate of 0.9 after 50,000 iterations.

Later, we expanded the dilemma by adding a third player, requiring new strategies and a more complex state.

| Our Agent | Opponent 1 | Opponent 2 | Reward |
|-----------|------------|------------|--------|
| Cooperate | Cooperate  | Cooperate  | 4      |
| Cooperate | Cooperate  | Defect     | 0      |
| Defect    | Cooperate  | Cooperate  | 10     |
| Defect    | Defect     | Defect     | 2      |

The RL agent's performance improved with an average win rate of 1 after 25,000 iterations in this new environment.

---
