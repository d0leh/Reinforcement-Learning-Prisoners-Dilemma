# Reinforcement-Learning-Prisoners-Dilemma

Prisoners Dilemma

	Prisoners Dilemma is one of the most famous dilemmas out there, where two prisoners are taken into separate rooms unable to communicate and each one is given the choice to either stay silent or betray the other prisoner. If both prisoners choose to stay silent, then each will go to prison for one year. If one chooses to betray and the other chooses to stay silent, then the betrayer will go free and the other prisoner will go to jail for 3 years. If both betray each other then both will go to jail for two years. The following diagram shows a summary of our dilemma.
 
Figure 1.1 (Prisoners Dilemma confusion matrix)

	This dilemma has huge real-life applications, especially in politics and business. We got curious about how a reinforcement learning agent would actually respond to it, and what it thinks the best decision is.

	Q-Table was not feasible as we have 2^10 states, so we opted for Q-Deep. Also, we wanted to put our hands in dirty neural networks code anyway.

	We started by creating a set of strategies that a player can use when playing against our agent so our agent learns against one of our strategies listed above, not random choices. These strategies we picked are actual strategies that people made in Axelrod’s Tournament, a tournament made by Robert Axelrod in 1980, where he invited well-known game theorists to submit strategies. You can check it out here.

●	Tit for Tat
○	Cooperates first then copies opponent’s last move.
●	Cooperate
○	Always cooperate.
●	Random
●	Tit for Two Tat
○	Cooperates unless the opponent defected twice in a row.
●	Grudger
○	Cooperates until the opponent defects then always defects.
●	Pavlov
○	Win stay, Lose switch.
●	Soft Grudger
○	Cooperate until defect then defect 4 times then go back to cooperation.
●	Fortress
○	Defect unless the opponent's last two moves were cooperate
●	Alternate
●	Sneaky
○	Cooperate then copy the opponent's last move, if the opponent cooperates, Sneaky has a 10% chance of defecting.

	After that, we started building the environment for our agent. The state is an array of our last 10 moves. The agent has two actions, whether to defect or cooperate (0 or 1). Each game consists of 200 rounds, our agent will try to win as many games as possible so we can then calculate its average win rate to indicate the performance of our model, where each win is a game in which the opponent gets to go to prison for more years than our agent.
Note that we tried to have our state be the opponent’s last 10 moves, however, we found that a state that changes based on the agent's action is more effective.

	The step function contains the changes happening in one round. It finds out the opponent's move and calculates the reward as follows: 
Our agent	Opponent	Reward
Cooperate	Cooperate	3
Cooperate	Defect	0
Defect	Cooperate	5
Defect	Defect	1
Then it makes the needed changes for the state such that it keeps updating for each round. In the reset function, we complete the needed initializations for a new game.

	Our agent then played with random actions and we tested the average win rate which was around 0.4 against our strategies. After that, we built our model and used DQNAgent from Keras as a start (we later wrote our own deep-q code) where our agent made it to a 0.9 average win rate and converged after 50,000 iterations. We used BoltzmannQPolicy because it provides a good balance between exploration and exploitation.

	We then wrote our own DQNAgent class, utilizing a memory buffer, and a high exploration policy in the beginning that decays over time.
Then we built a simple neural network that learns in mini batches, and tries to predict the next state of best quality based on its past experiences and chooses its action accordingly.

	We expanded our dilemma into adding a third player, so instead of having two prisoners, we now have three prisoners and each one is being investigated with in separate rooms, allowing each decision to make a difference in the other two prisoners state. In this environment, we had to change the strategies into the following:
●	Tit_for_tat_all
●	Tit_for_tat_any
●	Tit_for_two_tat_any
●	Tit_for_two_tat_all
●	Soft_grudger_any
●	Soft_grudger_all

	The state now is a 2d array where each row represents the last 10 moves of one of the opponents. The rewards have changed as follows:
Our agent	Opponent 1	Opponent 2	Reward
Cooperate	Cooperate	Cooperate	4
Cooperate	Cooperate	Defect	0
Cooperate	Defect	Cooperate	0
Cooperate	Defect	Defect	0
Defect	Cooperate	Cooperate	10
Defect	Cooperate	Defect	5
Defect	Defect	Cooperate	5
Defect	Defect	Defect	2

Our agent’s average win rate against opponents' strategy using random actions was 0.37. Then after training our model for 25,000 iterations and letting it converge, its average win rate increased to 1.

	In conclusion, our RL agent showed us that the optimal way to play Prisoners Dilemma is to try to catch the opponent off guard by defecting suddenly, and then playing defensively to maintain his lead.
