## Pacumen

![pacumen](http://testerstories.com/files/pacumen/pacman.png)

The name of this project is based on the name "Pac-Man" and the word "acumen." The word "acumen" generally refers to the ability to make good judgments and quick decisions. This is often done in a particular situation. In my case, that situation will be a Pac-Man game.

**Pacumen** is a modified implementation of the [Pacman AI project](http://ai.berkeley.edu) developed at UC Berkeley. This is a project that allows you to provide customized Pac-Man variations and then apply learning algorithms to those variations. This project was updated by my dad and was renamed ["Pacumen"](https://github.com/jeffnyman/pacumen).

I'm using my dad's project as part of my studies into reinforcement learning. This is also being used for my science fair and regionals project.

### Algorithms

My science project was based on algorithms and those were not provided to me. I had to figure out how to write them. My dad taught a class on [algorithmic searching](http://testerstories.com/category/ai/) and I was able to use some of those ideas to understand how to get started. But my project doesn't rely on algorithmic searching. The reason is because my project relies on adversarial searching, which is where Pac-Man has to deal with ghosts.

So I had to learn how to write the algorithms for my science project on my own and based on my background research. Those algorithms can be found in my [agents_q_learning.py](https://github.com/zachnyman/pacumen/blob/master/agents_q_learning.py) file. In that file, you'll see the following agents:

* PacmanControlAgent
* PacmanExperimentAgent

The `PacmanControlAgent` was actually a type of `QLearningAgent`. And the `PacmanExperimentAgent` was a type of `PacmanControlAgent` but with extra features. So every agent was using a Q-learning algorithm.

The `PacmanExperimentAgent` ran all of my agents except for the control one. What differed was what features the experiment agent considered.

### Feature Extraction

All of my agents relied on feature extraction. All of how my features were extracted can be seen in the [feature_extractors.py](https://github.com/zachnyman/pacumen/blob/master/game/feature_extractors.py) file. There you'll see the following extractors:

* IdentityExtractor
* CoordinateExtractor
* TacticExtractor
* StrategyExtractor

### Example Execution

It was important for my project that the Pac-Man agents were not given a set of instructions. So there is no logic in my code that tells Pac-Man exactly what to do.

The whole point of my project is that different agents were implemented using a Q-learning algorithm. That algorithm would be used by the Pac-Man agent and would differ in the types of features it considered important about the game.

This is just like how a human who had never seen the game before would have to figure things out. They would look at the game board, try various actions, and start to learn what mattered and what didn't matter. Eventually that learning should allow them to get good at the game.

The video link below is an example execution of one of my agents that got good at playing the game after it was given time to practice: 

[Video of Pac-Man Q-Learning](http://testerstories.com/files/pacumen/pacumen-execute.mp4)

It's really important to understand that all of that game play emerged from the above agent algorithms and feature extractors. None of that is hard-coded anywhere. And that could not be the case because the ghosts operate randomly. So any exact instructions would likely fail since those instructions could not be written to predict what the ghosts would do.

### Goal

The goal of the project is to use Pac-Man as a problem-solving environment for experiments about general artificial intelligence and learning algorithms.

This is an environment that can easily map onto the general definitions of search problems and decision processes. These are what characterize most approaches to artificial intelligence.

Pac-Man consists of objects moving around on a set of squares that can be modeled as a grid. At any given time Pac-Man occupies a square and faces one of the four directions: north, south, east, or west. There may be walls in between the square or entire squares might be blocked by walls. Regardless, the location of Pac-Man is determined by the x and y coordinates of the grid as such:

![pacumen-grid](http://testerstories.com/files/pacumen/pacumen-grid.png)

### Usage

`Pacumen` provides a very basic Pac-Man game. In the game, you control the movements of Pac-Man using arrow keys on your keyboard. You can also use the traditional gamer WASD keys: w (up), s (down), a (left), d (right).

Go ahead and try it.

```
python pacumen.py
```

The goal of the project is write agent programs to control the actions of Pac-Man. That is, creating a Pac-Man agent. The project enables you to use different environments to try out your Pac-Man agent programs. For example, try these:

```
python pacumen.py --layout test_maze
```

```
python pacumen.py --layout tiny_maze
```

You can also vary the scale of the screen by using the "zoom" option as shown below:

```
python pacumen.py --layout tiny_maze --zoom 2
```

```
python pacumen.py --layout big_maze --zoom 0.5
```

### Moving Parts

Everything in this section is likely to be modified or consolidated at some point in the future. It's all accurate, but not presented in the best format.

#### Agents

There is an `Agent` class that is very basic but defines the mode of operation, which is that agents will be executing in the context of the game. Pac-Man is an agent. So are the ghosts. Note that in classic Pac-Man, Pac-Man is always agent 0. The ghosts are then given numbers from 1 up.

An agent instance must define a `get_action` method, which essentially dictates how the agent moves about in the game world. An agent instance may also define a `register_initial_state` method, which does what it sounds like: sets up an initial state for the agent, such as a particular position.

All agents will receive a `GameState` and must return an action it is of type `Direction`.

There is also the concept of an `AgentState`. These instances hold just what it sounds like: the state of an agent. This state includes aspects like configuration, speed, "scared" (in the case of ghosts), and so on.

#### Actions

There is an `Actions` class and it serves as a collection of static methods for manipulating move actions in the game world.

#### Rules

There is a `GameRules` class that serve as basic rules for managing the control flow of a game, deciding when and how the game starts and ends. A specific set of rules, in the `PacmanRules` class specifically govern how the pacman agent interacts with the environment under the basic game rules. Likewise, `GhostRules` dictates how ghosts interact with the environment.

Collectively these classes are in place to make sure that the game world has rules and that any relevant agents -- Pac-Man and the ghosts -- follow those rules.

### Configuration

A `Configuration` instance holds the (x,y) coordinate of an agent, along with its traveling direction. The convention for positions, like a graph, is that (0,0) is the lower left corner, x increases horizontally and y increases vertically. This means north is the direction of increasing y, or (0,1).

An important method here is `generate_successor()` which generates a new configuration reached by translating the current configuration by the action vector. This is a low-level call and does not attempt to respect the legality of the movement. It should be noted that actions are movement vectors.

#### Game Control

Three main classes are `Game`, `GameState` and `GameStateData`. A The Game instance manages the control flow, soliciting actions from agents.

The `run()` method for the game instance is the main control loop for game play. In this method, `self.state` is a GameState instance while `self.state.data` is a GameStateData instance.

The idea of `GameStateData` is that it generates a new data state from a layout array. If there is a previous state, the new state is generated by copying information from the predecessor state. This state data is wrapped up in the GameState.

The idea of the `GameState` is that it generates a new game state from game state data and thus from a layout array. If there is a previous state, the new state is generated by copying information from its predecessor.

It's important to understand that a GameState specifies the full game state, including the food ("pac dots"), capsules ("power pellets"), agent configurations and score changes. GameStates are used by the Game object to capture the actual state of the game and can be used by agents to reason about the game. Much of the information in a GameState is stored in a GameStateData object. The GameState provides accessor methods for getting to this data, as opposed to referring to the GameStateData object directly.

#### Layouts

A core concept in this repository is that of a layout, which serves as a representation of a Pac-Man game board.

A `Layout` instance manages the static information about the game board. The layout text provides the overall shape of the maze. Each character in the text represents a different type of object.

* P - Pacman
* G - Ghost
* . - Food Dot
* o - Power Pellet
* % - Wall

Any other characters are ignored.

One of the core methods is `process_layout_text()`. This method modifies coordinates from the input format to a more standard (x,y) convention.

Also important is the `process_layout_character()` method. For each individual character in a layout board, this method will update an appropriate data structure (agent_positions, walls, food, capsules) to reflect the presence of that character.

One thing that's interesting about the grids that make up the Pac-Man board is that the origin point (0,0) is considered to be the _bottom left_ of the grid, and not the _top left_. The `y` coordinate is considered the height of the grid while the `x` coordinate is the width of the grid.

#### Test Maze

Consider this `test_maze` layout:

    %%%%%%%%%%
    %.      P%
    %%%%%%%%%%

Here the `%` character represents a wall, the `.` represents a food dot (called "Pac Dots" in the game), and `P` indicates the Pac-Man character.

Here is how _Pacumen_ represents this:

    layout.height: 3
    layout.width: 10

    Walls:
    TTTTTTTTTT
    TFFFFFFFFT
    TTTTTTTTTT

    Dots:
    FFFFFFFFFF
    FTFFFFFFFF
    FFFFFFFFFF

    Pellets:
    []

    Agents:
    [(True, (8, 1))]

For the walls, notice how the `F` portions correspond only to those parts that are _not_ walls. For the dots, notice how the lone `T` is indicating that a food dot was found in only one spot in this representation. There are no power pellets in this maze so no location is recorded for them.

Incidentally, the above representation is generated internally by this kind of list of (y,x) coordinates:

    [2][0] [2][1] [2][2] [2][3] [2][4] [2][5] [2][6] [2][7] [2][8] [2][9]
      %      %      %      %      %      %      %      %      %      %

    [1][0] [1][1] [1][2] [1][3] [1][4] [1][5] [1][6] [1][7] [1][8] [1][9]
      %       .                                               P      %

    [0][0] [0][1] [0][2] [0][3] [0][4] [0][5] [0][6] [0][7] [0][8] [0][9]
      %      %      %      %      %      %      %      %      %      %

Entities like Pac-Man are treated as _agents_. The above shows that Pac-Man was found at an x position of 8 and a y position of 1.

#### Test Modified

Here's a modified maze, which is a variation on the `test_classic` maze:

    %%%%%
    % o %
    %.G.%
    % . %
    %. o%
    %   %
    %  .%
    %   %
    %P .%
    %%%%%

Here we have the addition of a ghost agent (represented by `G`) and two power pellets, represented by `o`. (Power pellets are things Pac-Man can eat which allow him to eat the ghosts.) Here is how _Pacumen_ represents this:

    layout.height: 10
    layout.width: 5

    Walls:
    TTTTT
    TFFFT
    TFFFT
    TFFFT
    TFFFT
    TFFFT
    TFFFT
    TFFFT
    TFFFT
    TTTTT
    
    Dots:
    FFFFF
    FFFFF
    FTFTF
    FFTFF
    FTFFF
    FFFFF
    FFFTF
    FFFFF
    FFFTF
    FFFFF

    Pellets:
    [(3, 5), (2, 8)]

    Agents:
    [(True, (1, 1)), (False, (2, 7))]

Here you can see the pellets and the ghost are both represented. Given that this board is a bit larger, you can see how the coordinates for the pellets and the agents are starting from the bottom left.

Regarding the "True" and "False" for the agents, these are simply used to indicate the "agent of concern" or "agent of control." In this case, Pac-Man is the agent that we are concerned with applying machine learning to and is the one that we want to control. The other agent does not fit into that category.
