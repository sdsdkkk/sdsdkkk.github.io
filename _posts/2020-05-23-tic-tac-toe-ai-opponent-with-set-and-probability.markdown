---
layout: content
title: "Tic-Tac-Toe AI Opponent with Set and Probability"
date: 2020-05-23 00:00:00
description: Tic-tac-toe AI implementation using probability-based weighed moves
---

[Tic-tac-toe](https://en.wikipedia.org/wiki/Tic-tac-toe) is a simple game played on a board with the objective of lining up a straight line of crosses or circles from one end to another. The board is usually with the size of 3x3 grids as drawn below.

<script type="text/tikz">
  \begin{tikzpicture}
    \draw (0, 8) rectangle (4, 12) node[pos=.5] {1};
    \draw (0, 4) rectangle (4, 8) node[pos=.5] {4};
    \draw (0, 0) rectangle (4, 4) node[pos=.5] {7};
    \draw (4, 8) rectangle (8, 12) node[pos=.5] {2};
    \draw (4, 4) rectangle (8, 8) node[pos=.5] {5};
    \draw (4, 0) rectangle (8, 4) node[pos=.5] {8};
    \draw (8, 8) rectangle (12, 12) node[pos=.5] {3};
    \draw (8, 4) rectangle (12, 8) node[pos=.5] {6};
    \draw (8, 0) rectangle (12, 4) node[pos=.5] {9};
  \end{tikzpicture}
</script>

All of the grids on the board are labeled with numbers 1-9 for easier referencing later.

Two players are taking turns to occupy the grids, and the player who successfully occupies three grids that form a horizontal, vertical, or diagonal line through the board wins the game.

I decided to work on formulating a simple AI to play the game with what I know at this point of time as an exercise, while also looking into other approaches to see how it can be improved. One of the approaches I found is [using minimax algorithm](https://towardsdatascience.com/tic-tac-toe-creating-unbeatable-ai-with-minimax-algorithm-8af9e52c1e7d), which I haven't really understood at this point. Also, I'm avoiding using [game tree](https://en.wikipedia.org/wiki/Game_tree) in the current implementation since I want to see how well it can go with just sets and naive probability-based weights.

In general, I'm going with the simple approach on building the AI logic for this tic-tac-toe game based on the concepts of set and probability. I'm using sets to model the win condition of the game which is the end goal the AI will aim to achieve, and I'm using probability to set the weights of each grids for the AI to decide its next move based on how many possible winning scenario the said grid is able to support.

# Win Condition

The game ends when a player declared as a winner or all of the grids are occupied. When all of the grids are occupied and there's no declared winner, the game ends in a draw.

A player can be declared a winner when the player's occupied grids manage to form a straight line of three grids, covering from one edge to the other edge of the board.

Given that the set of grids within the game is $$G = \{1, 2, 3, 4, 5, 6, 7, 8, 9\}$$, according to the position of the grids as drawn above we can have the following sets of grids forming the straight line as the winning condition of the game.

$$
W = \{\{1, 2, 3\}, \{4, 5, 6\}, \{7, 8, 9\}, \{1, 4, 7\}, \{2, 5, 8\}, \{3, 6, 9\}, \{1, 5, 9\}, \{3, 5, 7\}\}
$$

Given that the set $$A$$ is the set of grids occupied by the player A, player A is declared the winner of the game if for any $$w \in W$$ the winning set of grid $$w \subseteq A$$ in the end of player A's turn. Also for player B, given that the set $$B$$ is the set of grids occupied by the player B, player B is declared the winner of the game if for any $$w \in W$$ the winning set of grid $$w \subseteq B$$ in the end of player B's turn.

The game is declared a draw if $$A \cup B = G$$ and for $$w \in W$$ neither $$w \subseteq A$$ nor $$w \subseteq B$$ holds true.

## Player Moves

Player A starts with the set $$A = \{\}$$ as the set of grids under their occupancy. Player B also starts with the set $$B = \{\}$$ as the set of grids under their occupancy. Both players are taking turns to take an available grid $$g$$ from $$G - A - B$$ to be added to their set of occupied grids ($$A$$ or $$B$$, depending on which player's making the move).

In general, the players will try to occupy the most strategic grids in order to win the game. The following is the number of win scenarios involving each grids 1-9.

Grid | Win Scenarios |
-----|---------------|
1    | 3             |
2    | 2             |
3    | 3             |
4    | 2             |
5    | 4             |
6    | 2             |
7    | 3             |
8    | 2             |
9    | 3             |

The corner grids 1, 3, 7, and 9 can be used in 3 win scenarios each, while the center grid 5 can be used in 4 win scenarios, and the rest of the grids 2, 4, 6, and 8 can be used in only 2 win scenarios.

The player with the first move can have up to 5 grids under their occupancy, while the player with the second move can have up to 4 grids.

After each move, the players will weigh their next move based on the current configuration of the grids. They should aim to stop their opponent from winning the game, especially if the opponent is only one move away from winning.

# Implementation

I implemented the AI logic for the opponent's moves using the win scenarios $$w \in W$$ as a reference when weighing the strategic advantage of occupying a certain grid. Weights are set based on the table above counting the number of possible win scenarios to pursue for each grid. There are total 8 win scenarios as included in the set $$W$$.

The initial weakness of this implementation is that the AI doesn't recalculate the weight for all the grids in the board after each turn because I didn't implement the recalculation for all remaining grids based on the still-available winning conditions, but just for the grids that has been occupied and the grids that needs to be occupied when one move away from winning or losing. So the AI will keep having lower weights for grids 2, 4, 6, and 8 compared to the other grids even when the game has progressed to the point where choosing to occupy those grids are actually more advantageous than occupying the remaining corner grids (1, 3, 7, or 8).

I ended up updating the weight for the whole grid using the calculation of how many remaining available winning condition open to the AI can be supported by each remaining available grids.

As soon as a grid is open for the AI or its opponent's last move in order to win it will raise the grid's weight to maximum value in order to occupy that grid as soon as possible. This is done in order to secure a win or avoid losing when it has a good chance of happening in the next move. It will prioritize winning over avoiding a loss in the cases where it has to prioritize one over another on the next move.

It can be guaranteed to be beaten by the player if the player has the first move and start taking corner moves though, since the logic will prioritize occupying a grid which support the most possible winning scenarios so it will prioritize taking the center grid (5) whenever it's available. The AI doesn't really plan that much into the future, as it only plans for the next one move based on the win conditions it can still achieve according to its knowledge base.

The following is [the gist](https://gist.github.com/sdsdkkk/37734cf9f510c99a407ae03fe87af2ea) for the implementation, hosted on GitHub. The code is tested with Python 3.8.1.

<script src="https://gist.github.com/sdsdkkk/37734cf9f510c99a407ae03fe87af2ea.js"></script>

# References

[Tic-tac-toe](https://en.wikipedia.org/wiki/Tic-tac-toe)

[Tic Tac Toe - Creating Unbeatable AI](https://towardsdatascience.com/tic-tac-toe-creating-unbeatable-ai-with-minimax-algorithm-8af9e52c1e7d)

[Game tree](https://en.wikipedia.org/wiki/Game_tree)

[Simple tic-tac-toe AI implementation using statistics-based weights on grids for 3x3 board](https://gist.github.com/sdsdkkk/37734cf9f510c99a407ae03fe87af2ea)
