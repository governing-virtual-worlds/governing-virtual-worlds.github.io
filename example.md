# AI Culture
World: [Seed](www.seed-project.io)
Architect: Joshua Tan

## One-liner

Track and aggregate reward functions across a settlement in order to create settlement-wide behaviors.

##Pitch

In video games, machine learning, often in the form of reinforcement learning, is often thought of as a way of to “optimize” traditional game AI so that it is more efficient or competitive (e.g. pathing, planning in Starcraft, DOTA, chess). This is the wrong approach for Seed. Such applications of ML are technically difficult to realize in dynamic, unconstrained environments due to the difficulty of defining a reward function. They are also likely to be as expensive to implement as hand-coded traditional game AI. Further, high-performance character AI is not necessary to the expected gameplay of Seed.

However, a more systematic, scientific approach to designing the character AI can support game development by (1) providing enriched flags for expected character behavior and (2) allowing developers to optimize different aspects of the base AI in a modular way. In what follows, I will focus on a systematic development of reward functions, a traditional input to game AI.

In games like the Sims, each object (and character) comes with a piece of information that tells other characters what kind of benefit they can expect to receive by interacting with the object. The entire list of all such rewards comprises a reward function f : Objects -> R on all Seed objects. This setup defines the reward information as an input to the character AI, allowing modular development. Using this setup, along with some simple techniques from inverse reinforcement learning (IRL), we can implement a simple game mechanic that I will call “culture”. The two key ideas to culture are (1) to infer player models based on statistics of player behavior using techniques from IRL (and perhaps other, more direct methods), and (2) to treat reward functions as a portable data format; in particular, one can aggregate multiple reward functions. Aggregation across a colony implements culture: a player’s behavior has a subtle influence on the behaviors of his/her neighbor, via the redefined reward function.

In the short-term, simply systematizing the capture of player statistics and the construction of player models (in the form of rewards), will likely be of significant use to the game.

## Background

### Learning in game AI

Reinforcement learning is a technique in AI where an agent learns by interacting with an environment which provides it feedback in the form of “reward”. For any game state, the agent chooses an action, for which it receives a reward or score from the environment. The agent then learns to optimize its sequence of actions (or strategy) in a way that maximizes the (expectation of) rewards. This is quite similar to the way that traditional game AI uses utility functions, except that rewards are maintained outside the agent while utilities are modelled inside the agent. The goal of RL is to learn a strategy that achieves an optimal or goal state in the game’s state space, which is typically massive. A particular type of reinforcement learning, model-based RL, 

On the other hand, inverse reinforcement learning is a technique for learning a reward or utility function from observations of behavior (typically “expert” or human behavior). This has not, to my knowledge, been used in games specifically, though many game companies do construct scientific models of their players for business development purposes. Simple variants of imitation learning (the AI copies what the human does, without going through an intermediary reward function) have been implemented in games.

Reinforcement learning can significantly improve the interactivity, efficacy, and realism of single-character game AI, but it is only worth using if the additional player interactivity is built into and valued within the game design (e.g. Black and White). If the purpose of adding ML is only to build more effective or more competitive game AI (e.g. chess, Starcraft), then there is unlikely to be a substantial gameplay or cost benefit of using ML over hand-coding. We don’t want the character AI to be so smart that the game plays itself.

*Remark*: On the other hand, it is significantly harder to hand-code a functional player AI, and machine learning / reinforcement learning can make this task significantly easier. A player AI is an AI that simulates Seed players and how they control characters. While player AI is not necessary to the core game, it can automate aspects of quality assurance, and contribute to key aspects of gameplay (e.g. group management, company management where multiple players collaborate).

The main previous example of machine learning in games was in Black and White (BW). BW used traditional game AI (rule-based systems, decision trees) along with belief-desire-intention models and simple neural networks to create a responsive, learning, and highly interactive “creature” that served as the player’s pet. Here is a link to a (semi-technical) overview. Another, more game-design perspective on BW contends that the creature ML was a “sublime” breakthrough, but that the full game design (especially the level design) did not fully take advantage of the ML-enabled creature interaction; in a comment, a QA tester of the game remarked that the earlier, sandbox parts of the game were more popular than the scripted, mission-based parts of the single-player campaign.

A similar, more population-based approach to artificial life was developed in Creatures: “The agents (known as “creatures”) are intended as “virtual pets”... Each creature has a neural network responsible for sensory-motor coordination and behavior selection, and an “artificial biochemistry”... A Hebbian learning mechanism allows the neural network to adapt during the lifetime of a creature.” (Grand 1996)

### Interactivity in games

In settlement sims like Rimworld and Dwarf Fortress, characters are machines that take orders (see the work tab below). The player sits in the role of a manager. Each character has a relatively thin illusion of “personality” based on random lists of likes, quirks, and traits like “pessimist”, “likes shellfish”, or “gay”, but in general the character is fully-controllable by the player, no matter their traits.

Work assignment tabs in typical settlement sims like Rimworld or Dwarf Fortress can be reduced with more advanced AI.

Below is an overview of AI in different games, and how players interact with them. Not mentioned are examples of “AI as director”, which helps to author player experiences.

Table from http://julian.togelius.com/Treanor2015AIBased.pdf

## MVP Specification

TBD.

Some observations about culture:
1. The gameplay mechanic is modular. Culture complements and interacts with other game mechanics, including colony laws and work management tabs. Importantly, it sits on top of, and does not replace, the traditional game character AI.
2. While I argued against standard optimization approaches for character AI, in principle, this mechanic could also be used to optimize the reward functions / reward data on all objects, for all characters (i.e. not necessarily localized to a colony). For example, more complex behaviors can be learned by optimizing or “filling in” reward functions gleaned from IRL.
3. Culture differentiates colonies and makes them seem “alive”. By changing reward values, characters in different colonies will tend to perform different behaviors in similar situations. Different colonies will naturally evolve different cultures (and potentially different bonuses for different behaviors, though this is not implemented directly by learning). For example, killing becomes okay for a raiding colony, but not okay for peaceful ones. Cooking your cat is okay in a colony populated by Chinese players, and not okay for one populated by German players.
4. Culture is a battleground: players can concretely and directly contribute to their local culture and ethics, or fight against it… with great difficulty.
5. Culture is fungible and portable. Reward functions, once defined, can be combined, divided, and transformed in different ways. For example, we can set “traveling” characters to behave either like a normal character in the colony they’re visiting—“when in Rome, do as the Romans do”—or like a character from their own colony. You can also use these learned reward functions to define character roles, on top of traditional work management tabs in settlement sims.

## Bibliography
Excellent list of AI-based design patterns in games + examples: http://julian.togelius.com/Treanor2015AIBased.pdf 
Deep reinforcement learning from human preferences: https://arxiv.org/abs/1706.03741
Survey of imitation learning: https://dl.acm.org/citation.cfm?id=3054912 
Overview article of AI in game production aka “AI as designer/producer”: https://www.cc.gatech.edu/~riedl/pubs/cig13.pdf
An authoring tool for character AI, but doesn’t really use learning: https://spiritai.com/product/character-engine/
Testing platform for reinforcement learning algorithms. https://gym.openai.com/
Creatures: http://mrl.snu.ac.kr/courses/CourseSyntheticCharacter/grand96creatures.pdf
Serious Games for Immersive Cultural Training: Creating a Living World: http://utdallas.edu/~maz031000/res/IEEE_Article.pdf
https://www.aaai.org/Papers/Symposia/Spring/1999/SS-99-02/SS99-02-017.pdf
Examples of best-in-class video game AI: https://www.gamasutra.com/view/news/269634/7_examples_of_game_AI_that_every_developer_should_study.php
Human-level control through deep reinforcement learning: http://www.davidqiu.com:8888/research/nature14236.pdf

## Appendix: Other potential uses of AI+ML

AI+ML as a service:
1. Test automation for quality assurance, e.g. Eggplant.io, or 3rd party tools in Unity
2. AI-based animation is easier, less tedious, saves money
3. Analyzing and optimizing the monetization strategy
4. Procedural generation of art based on artist input
5. Community management (https://spiritai.com/product/ally/)

AI+ML in the game:
1. Inspiration for the tech tree
2. AI as director: as in Rimworld or Left4Dead, that deliver personalized game experiences for players and colonies
  A.k.a. interactive storytelling using plot fragments, author modeling, dynamic difficulty adjustment
  Text-based versions, e.g. Versu, Spirit AI, “combinatorial narrative systems” in Icebound
3. AI as player: procedurally generate character group behavior and “social magic”
4. Procedurally-generated city plans that are locally customizable by players
5. Targeted optimization of particular tension mechanics


