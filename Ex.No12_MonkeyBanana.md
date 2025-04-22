# Ex.No: 11  Planning –  Monkey Banana Problem
### DATE: 22/04/2025                                                                      
### REGISTER NUMBER : 212222040075
### AIM: 
To find the sequence of plan for Monkey Banana problem using PDDL Editor.
###  Algorithm:
Step 1:  Start the program <br> 
Step 2 : Create a domain for Monkey Banana Problem. <br> 
Step 3:  Create a domain by specifying predicates. <br> 
Step 4: Specify the actions GOTO, CLIMB, PUSH-BOX, GET-KNIFE, GRAB-BANANAS in Monkey Banana problem.<br>  
Step 5:   Define a problem for Monkey Banana problem.<br> 
Step 6:  Obtain the plan for given problem.<br> 
Step 7: Stop the program.<br> 
### Program:
```
% Initial dynamic state
:- dynamic at/2.
:- dynamic hasknife/0.
:- dynamic hasbanana/0.
:- dynamic onbox/0.
:- dynamic clear/1.

% Initial setup
reset_state :-
    retractall(at(_, _)),
    retractall(hasknife),
    retractall(hasbanana),
    retractall(onbox),
    retractall(clear(_)),
    assert(at(monkey, room1)),
    assert(at(box, room2)),
    assert(at(knife, room3)),
    assert(at(banana, center)),
    assert(clear(box)).

goto(From, To) :-
    at(monkey, From),
    retract(at(monkey, From)),
    assert(at(monkey, To)),
    format('Monkey moves from ~w to ~w~n', [From, To]).

push_box(From, To) :-
    at(monkey, From),
    at(box, From),
    retract(at(box, From)),
    assert(at(box, To)),
    retract(at(monkey, From)),
    assert(at(monkey, To)),
    format('Monkey pushes the box from ~w to ~w~n', [From, To]).

climb :-
    at(monkey, Loc),
    at(box, Loc),
    clear(box),
    assert(onbox),
    retract(clear(box)),
    writeln('Monkey climbs on the box').

get_knife :-
    at(monkey, Loc),
    at(knife, Loc),
    assert(hasknife),
    retract(at(knife, Loc)),
    writeln('Monkey picks up the knife').

grab_bananas :-
    at(monkey, Loc),
    at(banana, Loc),
    hasknife,
    onbox,
    assert(hasbanana),
    writeln('Monkey grabs the bananas').

plan :-
    reset_state,
    goto(room1, room3),
    get_knife,
    goto(room3, room2),
    push_box(room2, center),
    climb,
    grab_bananas,
    hasbanana -> writeln('Goal Achieved: Monkey has the bananas!'); writeln('Goal Failed').
```
### Output:
![image](https://github.com/user-attachments/assets/58632492-d4ba-4f71-9a3c-1d7622152772)

### Result:
Thus the plan was found for the initial and goal state of given problem.
