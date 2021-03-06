# Graphs
A set of implementations of graphs, grids and traversal (path finding) algorithms, in C#. 

---------------------------------------
## What does this do?
This library is intended to provide path finding algorithms and graphs to use them with in a cohesive and straight-forward way. 

---------------------------------------
## Planned features
This library is still heavily work in progress and as such is entirely subject to change. Among the planned features are: 
* Hexagonal grids
* Polygonal (arbitrary) graphs
* Additional A* variants
    * Beam search
    * Iterative deepening
    * Dynamic weighting
    * Bandwidth search
    * Bidirectional search
    * Dynamic A* and Lifelong Planning A*
    * Jump Point Search
    * Theta*
* Easy to use path finding for non-class bound (int[,]) grids

---------------------------------------
## Usage
In order to use this library, you can choose one of two approaches. Either create a simple grid (in the form of a two dimensional array) from integer values or use one of the grid classes offered by this library. 
* The advantage of the classes is that they take work off your shoulders and provide a means of marking cell traversal costs. 
* The disadvantage of the classes is an increase in memory requirement. 

### Creating your own grid
```C#
    // Create your grid. 
    int width = 20;
    int height = 20;
    int tileImpassable = 1; // Acts as a non-walkable tile. 
    int[,] grid = new int[width, height]; // The grid of walkable tiles. 

    // Define some impassable tiles. 
    for (int i = 0; i < height - 8; i++)
    {
        grid[10, i] = tileImpassable;
    }
    // Define some difficult terrain. 
    for (int i = 0; i < height / 2; i++)
    {
        gridCost[15, i + height / 2] = 5;
    }

    // Define start and goal positions. 
    Point pntStart = new Point(1, 1);
    Point pntGoal = new Point(18, 5);
```

Using Breadth-First-Search
```C#
    IEnumerable<Point> path = BreadthFirstSearch.GetPath(pntStart, pntGoal, grid, tileImpassable);
```

Using Dijkstra's algorithm
```C#
    IEnumerable<Point> path = Dijkstra.GetPath(pntStart, pntGoal, grid, gridCost, tileImpassable);
```

Using A*
```C#
    // With diagonal traversal. 
    IEnumerable<Point> path = AStar.GetPath(pntStart, pntGoal, grid, gridCost, tileImpassable);
    // Without diagonal traversal. 
    IEnumerable<Point> path = AStar.GetPath(pntStart, pntGoal, grid, gridCost, tileImpassable, -1.0F);
```

### Using the SquareGrid class
```C#
    int width = 20;
    int height = 20;
    Size sizeTile = new Size(10, 10);
    SquareGrid grid = new SquareGrid(width, height, sizeTile);

    // Define some impassable tiles. 
    for (int i = 0; i < height - 8; i++)
    {
        grid.GetAt(10, i).impassable = true;
    }
    // Define some difficult terrain. 
    for (int i = 0; i < height / 2; i++)
    {
        grid.GetAt(10, i).cost = 5;
    }

    // Define start and goal positions. 
    Point pntStart = new Point(1, 1);
    Point pntGoal = new Point(18, 5);
```

Using Breadth-First-Search
```C#
    IEnumerable<SquareCell> path = BreadthFirstSearch.GetPath(pntStart, pntGoal, grid);
```

Using Dijkstra's algorithm
```C#
    IEnumerable<SquareCell> path = Dijkstra.GetPath(pntStart, pntGoal, grid);
```

Using A*
```C#
    // With diagonal traversal. 
    IEnumerable<SquareCell> path = AStar.GetPath(pntStart, pntGoal, grid);
    // Without diagonal traversal. 
    IEnumerable<SquareCell> path = AStar.GetPath(pntStart, pntGoal, grid, -1.0F);
```

After you got your path, you can iterate over the IEnumerable returned to you. The first element in the IEnumerable will be the first node of the path. 
For example, if you have an AI agent that is supposed to traverse the path, you cache the path (perhaps on the agent object itself) and set the agent's current position to the next node in the path. Then remove that node from the path and repeat until there are no more nodes in the path. 

---------------------------------------
## Credit
* BlueRaja - [Priority Queue in C#](https://github.com/BlueRaja/High-Speed-Priority-Queue-for-C-Sharp "CSharp Priority Queue")
* Amit - [Mountains of great articles](http://theory.stanford.edu/~amitp/GameProgramming/ "Pathfinding Articles")