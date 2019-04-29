
---
COMP110-SP19 - Problem Solving Assignment 7
---

## Due: Monday, May 13 11:59PM

## Required Background:

Reading from textbook: Chapters 1 - 6 and 11 - 12.


## Assignment Overview:

This assignment will give you practice using and understanding inheritance and
polymorphism in Python.
Specifically, you will be designing several classes that will create
characters in a game called _Critters_.
Critters was originally developed in Java at the University of Washington by
Stuart Reges and Marty Stepp, but USD was the first to do it in Python!

This assignment is out of **20 points**.
Point breakdowns will be developed.  


As always, commenting and formatting is an important part of the code you
write.
All classes and methods you write should contain docstring comments.
Failure to comment your code or having poor formatting will result in a 
penalty being assessed.


## Initial Setup

You will need to get the starter code 
using Git.

1. In VS Code, open the command palette and select the "Git Clone" option.

1. When prompted for the repository URL, enter the following, 

	https://github.com/comp110-sp19/psa7.git

1. Choose the "Open Repository" option 


## Program Behavior

We will provide you with several classes that implement a graphical simulation
of a 2D world with many animals moving around in it.
You will write a set of classes that define the behavior of those animals.
Different kinds of animals move and behave in different ways.
As you write each class, you are defining those unique behaviors for each
animal.

The critter world is divided into cells with integer coordinates.
The world is 60 cells wide and 50 cells tall.
Like images, the upper-left cell has coordinates (0,0).



### Movement

On each round of the simulation, the simulator asks each critter object which
direction it wants to move.
Each round a critter can move one square north, south, east, west, or stay at
its current location.
The world has a finite size, but it wraps around in all four directions.
For example, moving east from the eastern-most (i.e. right) edge brings you
back to the western-most (i.e. left) edge.
**You might want your critters to make several moves at once using a loop, but
you can't.**
The only way a critter moves is to wait for the simulator to ask it for a
single move and return that move.


### Fighting

As the simulation runs, animals may collide by moving onto the same location.
When two animals collide, if they are from different species, they fight.
The winning animal survives and the losing animal is removed from the game.
Each animal chooses one of **`Attack.ROAR`**, **`Attack.POUNCE`**, or
**`Attack.SCRATCH`**.
Each attack is strong against one other attack (e.g. roar beats scratch) and
weak against another (roar loses to pounce).
The following list summarizes the choices and which animal will win in each
case.

- `ROAR` beats `SCRATCH`
- `SCRATCH` beats `POUNCE`
- `POUNCE` beats `ROAR`

If the animals make the same choice, the winner is chosen at random.

### Mating

If two critters of the same species collide, they "mate" to produce a baby.
Once critters begin to mate, they stay at the same location for a set
gestation period.
The simulator indicates critters are in a gestation period by displaying
"&lt;3" over top of the critter.

Assuming they are not killed in a fight during their gestation period, at the
end of the period a new baby critter of the same type will appear in the world
at a random location.


### Eating

The simulation world also contains food for the animals to eat (represented by
the period character, `'.'`).
There are pieces of food on the world initially,
and new food slowly grows into the world over time.
As an animal moves, it may encounter food, in which case the simulator will
ask your animal whether it wants to eat it.
Different kinds of animals have different eating behavior; some always eat,
and others only eat under certain conditions.

After eating twice, a critter will be put to "sleep" by the simulator for a
small amount of time.
(Indicated by a "zzz" appearing over them in the world.)
While asleep, animals cannot move; if they enter a fight with another animal,
they will always lose.


### Scoring

The simulator keeps a score for each class of animal, shown on the right side of
the screen.
A class's score is based on how many animals of that class are alive, how much
food they have eaten, and how many other animals they have killed.


## Running the Simulator

To run the simulator, enter the following command into your VS Code terminal.
(Replace `python3` with `python` if you are using Windows.)

```bash
python3 critters.py
```

The bottom of the screen contains controls. The "Start" button will start the
simulation and continually run turns until you hit "Stop".
The "Tick" button does only 1 turn and will therefore be useful for debugging.
There is also a slider to control turn speed: the default turn speed is 0.1
seconds per turn but you can adjust it from 1 second per turn all the way down
to 0.03 seconds per turn.

The starter version of each of the critter classes all use the basic (read:
boring) behavior of standing still and not eating.
Therefore, before you make any changes, running the simulator will result in a
pretty boring simulation filled with a bunch of "?" critters and no action.
Don't worry: things will get much more interesting once you start implementing
each critter type!


## Provided Files:

Each class you'll write will inherit from a class named `Critter`.
Inheritance makes it easier for our code to talk to your critter classes, and
it helps us be sure that all your animal classes will implement all the
methods we need.
However, to complete this assignment you don't need to understand much about
inheritance.
Your class headers indicate the inheritance by adding "`(Critter)`" after the
name of the class, as demonstrated below.

```java
class Bear(Critter):
```

The Critter class contains the following methods, which you must write in each
of your classes:

- `def eat(self)`

	When your animal encounters food, our code calls this on it to ask whether
	it wants to eat (True) or not (False).

- `def fight(self, opponent)`

	When two animals move onto the same square of the grid, they fight.
	When they collide, our code calls this on each animal to ask it what kind
	of attack it wants to use in a fight with the given opponent.

	The `opponent` parameter is the string representation of the critter you
	are fighting (e.g. "B" for bear).

- `def get_color(self)`

	Every time the board updates, our code calls this on your animal to ask it
	what color it wants to be drawn with.

- `def get_move(self, neighbors)`

	Every time the board updates, our code calls this on your animal to ask it
	which way it wants to move (assuming it is not sleeping or gestating).

	The `neighbors` parameter will be a dictionary that maps a direction (e.g.
	`Direction.SOUTH`) to a string represention of the critter in that
	direction (e.g. "B" for a bear).
	If there is no critter in that direction, the value will be `None`.

- `def __str__(self)`

	Every time the board updates, our code calls this on your animal to ask
	what letter it should be drawn as.
	This method is used for the parameters for both the `fight` and `get_move`
	methods.

Just by writing "`(Critter)`" as shown above, you receive a default version of
these methods.
The default behavior is as follows.

- To never eat
- To always forfeit in a fight
- To use the color black
- To always stand still (a move of `Direction.CENTER`)
- A `__str__()` method that returns `"?"`.

If you don't want this default, rewrite (i.e. override) the methods in your
class with your own behavior.
For example, below is a critter class `Stone`.
Stones are displayed with the letter "S", gray in color, never move, never eat,
and always ROAR in a fight.
Your classes will look like this class, except with fields, a constructor, and
more sophisticated code. Note that the `Stone` does not need an `eat` or
`get_move` method; it uses the default behavior for those operations.

```python
class Stone(Critter):
	"""
	Class that represents a type of critter known as a stone.
	A stone will display as a gray 'S', and will always use Attack.ROAR in a
	fight, but otherwises uses the default Critter behaviors.
	"""

	def fight(self, opponent): 
		return Attack.ROAR

	def get_color(self):
		return "gray"

	def __str__(self):
		return "S"
```

## Running the Simulator:

When you press the "Start" button on the simulator, it begins a series of turns.
On each turn, the simulator repeats the following steps for each animal in the
game:

- Move the animal once (calling its `get_move` method), in random order.
- If the animal has moved onto an occupied square, fight! (i.e. call both
  animals' `fight` methods)
- If the animal has moved onto food, ask it if it wants to eat (call the
  animal's `eat` method)

After moving all animals, the simulator redraws the screen, asking each animal
for its `__str__` and `get_color` values.

<!--
It can be difficult to test and debug this program with so many animals on such
a large screen.
We suggest using a smaller game world and fewer animals (perhaps just 1 or 2
of each species) by adjusting the game's initial settings when you run it.
There is also a "Debug" checkbox that, when checked, prints a large amount of
console output about the game behavior.
-->


## Creating Critters

You will need to write the following classes, from scratch.

1. **Bear**: Bears are always hungry, scratch when fighting, and either move
   north or west.
2. **Lion**: Lions get hungry after they fight, they either roar or pounce, and
   they move in circles.
3. **Cheetah**: Cheetahs have a fixed amount of food they can eat, either
   scratch or pounce, and move randomly.
4. **Torero**: Each Torero is unique; you will decide the behavior of your own
   Torero!

Below is a more detailed description of the requirements for each of these
classes.


### Bear

- **constructor**: `def __init__(self, location, is_grizzly)` 
	- Note: Don't forget to call your parent's constructor, passing it the
	  `location` you were given.
	  Calling your parent's constructor (i.e. `__init__`) is discussed in
	  Section 12.4.3 in your textbook so read that section and the
	  corresponding code in Listing 12.5.
- **color**: "brown" for a grizzly bear (when `is_grizzly` is True), and "snow" for
  a polar bear (when `is_grizzly` is false) 
- **eating behavior**: Always returns `True` 
- **fighting behavior**: Always scratch (`Attack.SCRATCH`) 
- **movement behavior**: Alternates between north and west in a zigzag pattern
  (first north, then west, then north, then west, ...) 
- **string representation**: "B" 

The Bear constructor accepts a parameter representing the type of bear it is:
`True` means a grizzly bear, and `False` means a polar bear. Your Bear object should
remember this and use it later whenever `get_color` is called on the `Bear`. If the
bear is a grizzly, return a brown color ("brown"), and
otherwise a white color ("snow").


### Lion

- **constructor**: `def __init__(self, location)` 
- **color**:  "goldenrod3" 
- **eating behavior**: Returns True if this Lion has been in a fight since it
  has last eaten (if `fight` has been called on this Lion at least once since the
  last call to `eat`). 
- **fighting behavior**: if opponent is a Bear ("B"), then roar (Attack.ROAR);
  otherwise pounce (Attack.POUNCE). 
- **movement behavior**: First go south 5 times, then go west 5 times, then go
  north 5 times, then go east 5 times (a clockwise square pattern), then repeat.
  
- **string representation**: "L" 

Think of the Lion as having a "hunger" that is triggered by fighting. Initially
the Lion is not hungry (so `eat` returns False). But if the Lion gets into a fight
or a series of fights (if fight is called on it one or more times), it becomes
hungry. When a Lion is hungry, the next call to `eat` should return True. Eating
once causes the Lion to become "full" again so that future calls to eat will
return False, until the Lion's next fight or series of fights.


### Cheetah

- **constructor**: `def __init__(self, location, hunger)` 
- **color**: "red" 
- **eating behavior**: Returns True the first `hunger` times it is called, and
  False after that 
- **fighting behavior**: If this Cheetah is hungry (if eat would return true),
  then scratch (Attack.SCRATCH); else pounce (Attack.POUNCE)  
	- NOTE: Based on your code, calling `eat()` may change the hunger status. So
	  it is **not** wise to call `eat()` to check if the cheetah is hungry.
- **movement behavior**: Moves 3 steps in a random direction (north, south,
  east, or west), then chooses a new random direction and repeats (a discussion
  of how to generate random numbers is found below) 
- **string representation**: The number of pieces of food this Cheetah still
  wants to eat, as a string (i.e. "4"). 

The Cheetah constructor accepts a parameter for the maximum number of food this
Cheetah will eat in its lifetime (the number of times it will return true from a
call to eat). For example, a Cheetah constructed with a parameter value of 8
will return true the first 8 times eat is called and false after that. Assume
that the value passed for hunger is non-negative.

The `__str__` method for a Cheetah should return its remaining hunger, the
number of times that a call to eat would return true for that Cheetah. For
example, if a new Cheetah(5) is constructed, initially that Cheetah’s `__str__`
method should return "5". After eat has been called on that Cheetah once, calls
to `__str__` should return "4", and so on, until the Cheetah is no longer
hungry, after which all calls to `__str__` should return "0". Recall that you
can use the `str()` function to convert a number to a string.


### Torero

- **constructor**: `def __init__(self, location)` (must **NOT** have any other
  parameters!) 
- ***All other behavior***: you decide! (see below):
	- **color:** 
	- **string representation:** 
	- **fighting behavior** 
	- **movement behavior** 
	- **eating behavior** 

- **Requirement**: Your Torero must be visually (when you play the game)
  distinguishable in some way from the other critters (at least SOME of the
  time). We need to be able to see if you are winning!
  Also: your `__str__` method should return only a single character (e.g. "T"
  is fine but "Ole" is not).

	All of your behaviors (other than color and string representation) must be
	unique and non-trivial.
	You will **not** receive credit it you simply copy the behavior of other
	critters or if you make only a trivial modification.

You will decide the behavior of your Torero class.
Part of your grade will be based upon writing creative and non-trivial Torero
behavior.
The following are some guidelines and hints about how to write an interesting
Torero.

There are additional instance variables that each critter can use through
inheritance from the Critter class.
Your Torero may want to use these instance variables to guide its behavior.
None of these below are needed for Bear, Lion, or Cheetah.

- `self.x`, `self.y`: The x and y location of the critter in the world.
- `self.world_width`, `self.world_height`: The width and height of the world.



Your Torero’s fighting behavior may want to utilize the parameter to the fight
method, `opponent`, which tells you what kind of critter you are fighting against
(such as "B" if you are fighting against a Bear).

Your Torero can return any single character you like for `__str__` (e.g. "T"
or "=") and any color from `get_color` (e.g. "blue").
Each critter's `get_color` and `__str__` are called on each simulation round,
so you can have a Torero that displays differently over time.
The `__str__` text is also passed to other animals when they fight your
Torero; you may want to try to fool other animals.





## Generating random numbers

In this assignment, at least for the Cheetah class, you will need to generate
random values.
Python's `random` module makes this easy.
You may want to use one of the following functions of the `random` module:

- `random.randrange(start, stop, step)`: Same basic operation of `range`
  except that is chooses a random value from the range.
  For example `random.randrange(0, 10, 3)` will pick a random number from 0,
  3, 6, and 9.

- `random.random()`: Returns a random floating point number between 0.0 and
  1.0.

- `random.choice(sequence)`: Randomly picks one of the items from the given
  sequence (e.g. a list). For example `random.choice(["foo", "bar", "meow"])`
  would randomly return either "foo", "bar", or "baz".
  Using this function with a list of `[Direction.NORTH, Direction.SOUTH, ...]`
  could be very useful.


## Testing/Grading

You can test your Critters by running them in the Critter coliseum. To do this,
simply run the following command from your VS Code terminal (replacing python3
with python if you are using Windows)::

```bash
python3 critters.py
```

We will be releasing a detailed testing script.

<!--
When you run CritterMain, it will scan the current folder and look for any class
files that extend the Critter class. Then it will ask you which of those classes
you would like to load into the coliseum. The best way to test your individual
critters is to load them one at a time into the coliseum and see if they exhibit
the correct behavior.
-->

## Submission Instructions
TBD
