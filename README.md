# Strategy-Determination-Program
This repository provides supplemental code for the attached preprint titled _A Computational Solution to the Game of Cycles_. Using a tabular representation of a game board, the program analyzes every possible outcome of the game and outputs a guaranteed winning strategy for either player 1 or player 2.

We now detail necessary concepts for designing program inputs and interpreting the output. Note that the attached preprint can be consulted for a theoretical basis of the proposed methods, and we welcome questions about methods and applications if they arise.

Constructing a Game Table:
-------------------------
As described, the program requires the user to input a tabular representation of a game board. For boards with _m_ vertices and _n_ faces, we propose a m x (n + 1) _game table_ that can be constructed as follows:
1. Assign a unique number $1, 2, \ldots, m$ to every vertex on the board.
2. Assign an indexed variable $f_1, f_2, \ldots, f_n$ to every face on the board; assign $f_{n+1}$ to be the outer face.
3. Construct an empty table with the faces labeling the columns and the vertices labeling the rows, in ascending order.
4. For every entry in the table, if the vertex labeling the row is on the non-outer face labeling the column, fill the entry with the _number of the vertex immediately clockwise around the face to the vertex labeling the row_. If the vertex is on the face multiple times, as in Example 5 of the attached preprint, enter the vertices as a comma-separated list. The entries in the last column, corresponding to the outer face, will be filled in the counterclockwise direction. Though players do not win by creating a cycle around the outer face, those edges must be represented in the rows of their respective vertices because they enable our code to detect (and thus avoid creating) sinks and sources.
5. All other entries are set to zero.

When formatting the table for program input, we naturally require at two dimensions. Because game tables such as Example 5 of the attached preprint require comma-separated lists of vertices in a two-dimensional table entry, we introduce a third dimension to the table that assigns each entered vertex a unique index. These unique indices are critical for the program when accessing an individual vertex. The final game table should then be stored in the variable _table_ in the second block of the program, which currently contains the game table of Example 5. For further demonstration of game table construction, the file _Game Table Examples_ in the repository includes various game boards and their respective game tables.

Program Output:
---------------
Once a game table has been inputted into the program, the program will output the player with a guaranteed winning strategy in cell 8. The following cell of code will indicate the correct moves to the victorious player, as described in the attached preprint. The last four lines in the final cell of code can be uncommented to output a directed graph of all possible gameplays, with the nodes colored based on their _impartial game position_.

In addition to the final output, the program includes additional runtime updates. As the inputted game board becomes large, the program runtime will increase, so these updates offer valuable insight to program function and completion time during execution.

Versions:
---------------
python: 3.9.7

more-itertools: 8.10.0

networkx: 2.6.3

matplotlib: 3.4.3

matplotlib-inline: 0.1.2
