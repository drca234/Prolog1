%citypath takes the start, end, maze, and a path variable.
%if the start x is the same as the end x and the start y
%is the same as the end y, the solution is found unless
%there is a jenny blocking the square. If this is the case, 
%the puzzle is unsolvable and the program returns no. 

%Also good to note: The output will be in row, column format.

%Included in order to use nth1
:- use_module( library( listut ) ).

%This calls a version of the function that has a variable to store the current path.
citypath( X1, Y1, X2, Y2, Maze, Path ) :-
	CurrentPath = [ [ X1, Y1 ] ],
	citypath( X1, Y1, X2, Y2, Maze, Path, CurrentPath ).

%Checks to see we are at the destination, then if there is a Jenny at the destination, then returns the path if no Jenny.
citypath( X1, Y1, X2, Y2, Maze, Path, CurrentPath ) :-
	X1 == X2, Y1 == Y2, 
	!, validMove( X1, Y1, Maze ),
	reverse( CurrentPath, Path ).

%Tries to move in the X + 1 direction. Fails if that location was already explored, or if the location is invalid. 
citypath( X1, Y1, X2, Y2, Maze, Path, CurrentPath ) :- 
	NewX is X1 + 1, validMove( NewX, Y1, Maze ), 
	\+member( [ NewX, Y1 ], CurrentPath ), 
	append( [ [ NewX, Y1 ] ], CurrentPath, NewPath ), 
	citypath( NewX, Y1, X2, Y2, Maze, Path, NewPath ).

%Tries to move in the Y + 1 direction. Fails if that location was already explored, or if the location is invalid. 
citypath( X1, Y1, X2, Y2, Maze, Path, CurrentPath ) :- 
	NewY is Y1 + 1, validMove( X1, NewY, Maze ), 
	\+member( [ X1, NewY ], CurrentPath ), 
	append( [ [ X1, NewY ] ], CurrentPath, NewPath ), 
	citypath( X1, NewY, X2, Y2, Maze, Path, NewPath ).

%Tries to move in the X - 1 direction. Fails if that location was already explored, or if the location is invalid. 
citypath( X1, Y1, X2, Y2, Maze, Path, CurrentPath ) :- 
	NewX is X1 - 1, validMove( NewX, Y1, Maze ), 
	\+member( [ NewX, Y1 ], CurrentPath ), 
	append( [ [ NewX, Y1 ] ], CurrentPath, NewPath ), 
	citypath( NewX, Y1, X2, Y2, Maze, Path, NewPath ).

%Tries to move in the Y - 1 direction. Fails if that location was already explored, or if the location is invalid. 
citypath( X1, Y1, X2, Y2, Maze, Path, CurrentPath ) :- 
	NewY is Y1 - 1, validMove( X1, NewY, Maze ), 
	\+member( [ X1, NewY ], CurrentPath ), 
	append( [ [ X1, NewY ] ], CurrentPath, NewPath ), 
	citypath( X1, NewY, X2, Y2, Maze, Path, NewPath ).

%If the coordinate in question is out of range, this fails through one of the nth1 calls. 
%If the coordinate is valid, it checks for Jenny, and fails if she is there.
%If the coordinate is valid and there is no Jenny, the space can be moved to.
validMove( X1, Y1, Maze ) :- 
	nth1( X1, Maze, Column ), nth1( Y1, Column, IsJenny ), IsJenny == 0.