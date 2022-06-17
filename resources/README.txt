1. Circle the Cat (dragon)

=================================================
2.Written by: 
	Damian Tissembaum,  id= 340888122,  login= damianti
	Ori Vered,	        id = 315545400, login = orive

3.*********** General program description***********

This program is the game "Circle the Cat", done with a dragon instead of a cat

In this program, there is a map of tiles. These tiles are the places for a dragon to step
inside them and try to go outside the board, escaping from the user.
The user has to click on tiles trying to trap the dragon, preventing it to go out from the
map. 
The map has 121 tiles (11*11), whiche every even row is placed a bit left so that every tile has
at most 6 neighbours. So a step of the dragon is flying from the tile that it was placed to a neighbour
The dragon calculates its shortest way to an edge of the map after every click and try
to go out. If the dragon arrives to an edge, the user looses and the same level is started 
again. If the dragon does not have any move to do (and it is not placed on an edge of the map)
the user wins the level and the next level is showed.

The game has been written to play 3 levels. If the user wins them, he wins the game.
The game does not have any timer to finish a level, it just count the steps that the user has done.


The user has the option of:
    Reset the level: the program will show the same level that has been playing
    from the beggining
    Undo the last step: the dragon will move to its previous place and the steps will be reduced by one
    
Also, the user can see in which level he is playing

The program will generate levels using an algorithm that in each level will be at most 14 bussy tiles (where the cat cannot step on it)
and the next levels has more probability of haveing less bussy tiles

The program will create an interface using SFML graphics and sound

4.*********** Design ***********

The hole program will be controlled and runned by an object of the class Controller. it controlls 
the game by having an object of the class board (that has the board itself, a vector of vector of tiles),
an object of the class Dragon, the options like reset, levels (shows the levels), and undo.

Explaining about the main classes of the program:

    The class Controller has these members:
        - A board (an object of the class Board)
        - A dragon (an object of the class Dragon)
        - Two unique_ptr of InfoButton (a class that inherits from Button and is used to create
        a text that can be clicked) for the Reset and Undo options for the user to click
        - Two unique_ptr of Info (a class that shows information and has a number at the end that 
        is changed by a specific event) for the Levels and Steps information


    The class Board has these members:
        - A two dim std::vector of tiles (object from the class Tile that inherits from Button and is used
        to create the possible position of the dragon and the places where the user can click on (thats why it inherits from button))
        - A std::stack of std::pair of int in order to save the bussy tiles that has been set 
        as bussy during the game in chronological order so that the controller can do the Undo action (Ronaldinho)

    The class Dragon has these members:
        - A std::stack of std::pair of int in order to save the tiles that the dragon has 
        been placed during the game in chronological Ronaldinho order so that the controller can 
        do the Undo action (Ronaldinho)
        - A boolean called m_isMoving that is true if the dragon is moving from a tile to 
        another (when the dragon is moving the click from the user does not do anything)
        - Two vector2f: the m_lastPosition and the m_nextPosition of the dragon in order to move it
        - A RectangleShape m_dragonBody in order to use the animation
        - A bool m_faceRight used for the animation (if is true the face of the animation 
        of the dragon will be on the right side of it)
        - A float m_speed that is used to move with a certain speed the dragon
        - An object of the class Animation called m_animation that is used to do an animation to the dragon
        - A std::pair of int that represents the index position of the dragon called m_loc


In the hole game only one object of the class Board is created. So in every level finished or reseted, the
board object restart itself using the level that is been played on the moment (sent by controller or first level by default)

The Class Button is a virtual base class and the classes InfoButton and Tile inherit from it

There is always one and only one object of the class dragon playing, but every time there is a new start of a level (meaning that the user
loose, clicked restart or won the game) a new object of the class Dragon is created and the last object used is erased.
Every new object of the dragon is created, it is placed in the middle of the board (at the tile with row 5 and col 5)

The hole game is designed by indexes, meaning that every index has its position on the window

Every time the user clicks on a free tile, the board sets that tile as clicked (bussy), 
and then calculates which is the best next position for the dragon in order to get to an edge using BFS algorithm.

Then the controller sets the next position of the dragon and the dragon goes to there.

5.***********  Files ***********
main.cpp - main
macros.h - consts, enums, including of the program

Board.cpp - Board functions file
Board.h - Board declartion file

Button.cpp - Button functions file - button abstract base class
Button.h - Button declartion file

Controller.cpp - Controller functions file
Controller.h - Controller declartion file

Dragon.cpp - Dragon functions file
Dragon.h - Dragon declartion file

Info.cpp - Info functions file
Info.h - Info declartion file

InfoButton.cpp - InfoButton functions file
InfoButton.h - InfoButton declartion file

ResourceManager.cpp - ResourceManager functions file - singelton pattren
ResourceManager.h - ResourceManager declartion file

TempState.cpp - TempState functions file
TempState.h - TempState declartion file

Tile.cpp - Tile functions file
Tile.h - Tile declartion file

Utilities.cpp - Utilities funtions file
Utilities.h - Utilities declartion file

dragons.png - dragon sprite sheet image file
gameOver.png - game over image file
gamewin.png - game win image file
levelUp.png - game level Up image file
mainTrack.ogg- game sonf file
ALGER.TTF - font file

6. **********data structures***********
Board - vector<vector<shared_ptr<Tile>>> - Board matrix for TIles
      - stack<LocIndx> - stack of bussy cell's indexes for undo

Dragon - stack<LocIndx> - stack of last's cell's indexes for undo

ResourceManager - singelton pattern

TempState - image will show fixed time in screen

7. **********remarkable algorithms***********

calculate shortest way for Dragron to edge of Board:
we execute BFS algorithm for src Tile to one of Edge Tiles
the algorithm save parents founders to Restore BFS way and return first step to edge

random BFS neighbours search :
to make the game harder and unxpected we randomize the order of neighbours in BFS queue,
every cell in bfs add his neighbours to the queue by random order, so cat next step is unxpected

random Level Map:
each Level will randomize the bussy tile by set of cell indexes

Undo step:
we execute Undo option for a bonus with stack of cell indexes:
when user click on Undo Button, steps is decrease, move cat to last position from the stack,
and free the last bussy tile from stack.

   
8. ******bugs******
unkowns bugs.

9.*****comments*****
to make easy of code write we use with indexes(i,j) for all positons in the pgrogram
we execute with namespace alias that call LocIndx to represnt std::pair<i, j>
the function setTilePositon in Utilities.cpp convert indexes location to sf::Vector2f correctly postions

