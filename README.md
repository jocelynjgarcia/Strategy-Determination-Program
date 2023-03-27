# Strategy-Determination-Program
This repository provides supplemental Code for a research paper on the Game of Cycles that is currently in process. Using an inputted legal board, the program analyzes every possible outcome of the game and outputs a guaranteed winning strategy for either player 1 or player 2. Note that this repository is currently in its initial stages, so feel free to reach out if you have any questions as we refine the descriptions and write the research paper.

Constructing a Game Table:
-------------------------
As described, the program requires the user to input a legal game board. We developed a tabular representation of a planar graph called a game table that maintains an isomorphism of faces. Given a board in the Game of Cycles, we construct an m x n game table as follows:
- Assign a unique number to every vertex in the board
- Assign an indexed variable $f_1,f_2,\cdots,f_n$ to every face in the board, assigning a variable to the infinite (outer) face last
- Construct an empty table with the faces labeling the columns and the vertices labeling the rows, both in numerical order
- For every entry in the table, if the vertex labeling the row borders the face labeling the column, fill the entry with number of the vertex immediately clockwise around the face to the vertex labeling the row. If the vertex borders the face multiple times, then enter the vertices as a comma-separated list. The infinite face will be filled in the counterclockwise direction around the graph, producing a clockwise boundary with respect to the interior of the face.

Before detailing game play, a natural question arises regarding the inclusion of the outer face. The rules of the game do not allow players to win by cycling the outer face, but we must include its edges because of their implications on sinks and sources. The entries are part of a row representing a vertex and therefore play a role when avoiding sinks and sources, as will be described soon.

After the board is constructed, a game can then be played by marking non-zero entries as exclusively positive ($+$) or negative ($-$), giving edges a defined direction as seen in the example. If the entries are a comma-separated list, then add direction to one of the entries in the list. By marking an entry as positive (respectively, negative), the arrow is being marked from the row vertex to the entry vertex (the entry vertex to the row vertex). In other words, a positively (negatively) marked entry means that the edge is following (going against) the clockwise direction with respect to the face that determined the entries in step 4.

Because a given edge borders two faces, entries must be marked in pairs. Directed edges always run clockwise to one face and counterclockwise to the other, so when an edge is marked with respect to one face, then it must be marked in the opposite direction with respect to the other face. The paired entry is determined by examining the row of the entry vertex and finding the entry that matches the previous row vertex.

Since a row represents the edges adjacent to a vertex, all of the edges in a row cannot be marked in the same direction with respect to the vertex, meaning that they cannot all be directed towards or away from the vertex. In the context of the game table, this means that all non-zero values in a given row cannot be positive or negative. To answer the previous question on the infinite face inclusion, the removal of the entries would result in a missing edge around a vertex, changing the conditions for near-sinks and near-sources.

Since a column represents the edges around a face, a cycle is formed when all of the edges in a column are marked in the same direction, except for the last column representing the infinite face. In other words, all of the non-zero values in a column are given the same sign. As described above, positive signs represent clockwise cycles and negative signs represent counterclockwise cycles.

In the context of a game table, a Game of Cycles ends when all entries in a column are marked with the same sign (the creation of a cycle) or marking any pair of entries produces a row where all of the edges are marked with the same sign (only unmarkable edges left).

A brief outline of the program:
------------------------------
The algorithm builds a labeled directed graph of all possible boards using the following method:
- Using an inputted game table, determine sets of moves that create sinks/sources or cycles
- Generate all possible boards and remove boards with sets of moves that create sinks/sources 
- For every board $G$ without a cycle, add a directed edge to every board $G'$ that $G$ could become with one additional move
- Starting at the end of the game, label boards as P-positions (yellow below) or N-positions (dark grey)

The player with a winning strategy then uses the directed graph to always leave the board in a P-position, as described by impartial game theory.

Program Output:
---------------
When the program has finished running, the output will consist of the first moves that should be made using a list that represents the current state of the game. During the program, the paired entries of a game table that represent an edge are determined, which are then used to build a list that is the length of the number of edges on the board where each entry represents a move played on a given edge. The program steps through every value in every entry of the table. Because positive entries in the game table represent edges, the zero-based value most be nonnegative. The location of the value is then saved as the start point of the edge, and the end point of the edge is determined searching the row corresponding to the found value for the row number of the start point. We can then check if the pairing of startPoint and endPoint are already in the mapping by checking if the endPoint is already a key in the mapping dictionary. This would means that the endPoint occurred earlier in the table, and the current start point is the value attached to that key. If the pairing is not already in the mapping, then the startPoint-endPoint can be appended as a key-value pair to the mapping and appended as endPoint-startPoint in the reverseMapping. This representation reduces run time because it allows the program to work with one-dimensional lists instead of two-dimensional tables.

The creation of this ordered mapping allows a list that is the length of the mapping filled with 0, 1, or -1 to represent a board where entries 1,2,3,... of the list are matched with key-value pairs 1,2,3,... of the mapping. We establish this representation by stating that a zero represents and unmarked edge at the matching mapping edge, a 1 represents directed edge in the matched edge of the mapping marked in the positive direction of the key and the negative direction of the value. The opposite of the 1 and -1 is true for the reverseMapping, which we create so that a program does not need to check the value of every key in the mapping dictionary and instead look for the key in a reversed dictionary.

Because of this mapping, the edges of the directed graph from a given vertex must be found using the list of moves that represents the current game state. The final chunk of code in the repository allows the user to input a specific game state list and outputs the possible moves and their type. The game state is inputted by changing the value tuple(emptyBoard) to the tuple of the game state. Inputting the empty board demonstrates the possible first moves of the game. According to impartial game strategy, the player 1 has a guaranteed winning strategy if they can make a move to a p-position, otherwise player 2 has a guaranteed winning strategy.

Versions:
---------------
python: 3.9.7
more-itertools: 8.10.0
networkx: 2.6.3
matplotlib: 3.4.3
matplotlib-inline: 0.1.2
