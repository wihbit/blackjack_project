# Blackjack Project

## Overview
Creating an application in which players can play virtual blackjack alongside a blackjack-playing bot, while receiving AI-generated tips and suggestions based on their hand.

## Requirements
```
python==3.10.14
gradio==5.0.2
gym==0.26.2
numpy==1.26.4
pillow==10.4.0
tensorflow==2.10.0
```

## How to use
1. To use the interactive app, go to https://huggingface.co/spaces/wihbit/blackjack_gradio  
2. Start your first game by clicking 'Reset'. This will deal a blackjack hand to you, the dealer, and a trained bot.  
3. Each turn, decide if you will hit, stand, or split. Split is only available if you are dealt two of the same card (different suites)
    - A recommended choice will be shown to you
    - If you are able to split, you will have two separate hands to play. You will be prompted to choose to hit or stand on each one separately.
5. Click 'Submit' to lock in your choice.
    - Like in Vegas, the dealer will hit or stand based on a strict set of rules.  
    - The bot will hit or stand based on its learned programming.
    - Neither the bot nor the dealer are programmed to split
6. When the game is over, each player's outcome will be displayed.
7. Click 'Reset' to start a new game

## Using machine learning to train a model to play a game

- One of the goals of this project was to learn the basics of reinforcement learning and apply this concept to create a 'bot' that has 'learned' the game of Blackjack well enough to make sensible decisions on its own. 'Sensible' in this case meaning better than random guessing.
- One facet of reinforcement learning that we have not encountered so far is that we do not have a premade dataset to train a model on. There is technically a dataset, but the dataset creates itself in the process of simulating thousands of games.
- For this approach, we researched and implemented Q-learning, specifically deep Q-learning, because it is a good introduction to reinforcement learning, and well suited for simple problems such as Blackjack.

## Deep Q-Learning

- The 'Q' in Q-learning stands for 'quality', and is based on the 'Q function', which is meant to calculate the predicted 'quality' of a given action (such as hitting or standing in Blackjack).
- Q-learning is a type of reinforcement learning algorithm, where an agent learns to make decisions by interacting with an environment and receiving rewards or penalties. The goal of the agent is to learn a policy, which is a mapping from states to actions that maximizes the expected cumulative reward over time. In Q-learning, the agent maintains a Q-table, which stores the expected cumulative reward for taking each action in each state. The agent updates the Q-table based on the rewards it receives, and uses it to make decisions about which action to take in each state.
- Deep Q-Learning means that a neural network, not a Q-table, is calculating Q-values.
- Since Q-learning is being applied in the context of a game, we can make sense of the reinforcement learning terminology with these equivalences:
    - Environment = Game mechanics
    - Agent = Player (bot)
    - Reward = Points/winning
    - State = Status/circumstances of the game at a given moment
    - Policy = Strategy
    - Q value = Predicted reward for a move/play

- The Q-learning process can be summarized in these steps:
    1. A game is recreated in code
    2. A simulated player is coded (bot)
    3. The bot plays the game repeatedly
    4. Each play, the player makes random(ish) moves
    5. Rewards/penalties are given according to rules
    6. The bot continually updates its memory of what moves resulted in what rewards under what circumstances
    7. The bot increasingly favors moves with the highest predicted reward
    8. The bot eventually develops its own strategy (policy) of how to play

## Epsilon-Greedy
- Epsilon-Greedy refers to a reinforcement learning approach that balances an agent's exploration with its exploitation. 'Epsilon' refers to exploration, which is generally carried out by choosing actions at random. 'Greedy' refers to exploitation, which is generally carried out by choosing the action with the highest q-value.
- The approach is to start with an epsilon value of 1, meaning that the agent is 100% likely to choose a random action and evaluate the outcome.
- With each new simulated game, the epsilon value should gradually decrease, shifting the balance towards exploitation. For example, the next epsilon value might be 0.99, which means the agent is now 99% likely to choose a random action, and 1% likely to choose the action with the best q-value.
- By the end of training, the epsilon value should be low, but typically not 0. For example, ending with epsilon 0.1 means that it is only making random actions 10% of the time, and choosing the best q-value 90% of the time.
- Starting with high exploration (low exploitation) is necessary to build a baseline of q-values.
- Ending with high exploitation (low exploration) is necessary to confirm and refine the learned q-values

## gym/gymnasium
https://gymnasium.farama.org/
- 'gym' is a python library that contains many prebuilt environments, including one for Blackjack, for performing reinforcement learning, but also provides an API for design custom environments.
- gym was originally created by OpenAI, but OpenAI shifted focus away from it and stopped maintaining it. The Farama Foundation created a fork of it, called 'gymnasium', to continue improving and maintaining the project. gym and gymnasium are mostly identical, with gymnasium being backward-compatible, and having a few improvements and bug fixes.

## Application
- After training a bot to play Blackjack, we developed functions to create an application that is a playable version of Blackjack, in which a human player can play against the dealer, and the trained bot.
- Gradio was used to create a user interface for interacting with the application.
- Gradio apps can be launched locally. They can create temporary instances that can be shared over the web, but the app's backend processes would still happen locally, requiring the host machine to remain active.
- Instead of going that route, we chose to upload the Gradio application to Hugging Face
    - Hugging Face is able to host the entire application on their cloud service for free (with limited virtual CPU and memory)
    - This makes the application available 24/7 with no additional maintenance required from us
