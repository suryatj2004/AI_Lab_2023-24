# Ex.No: 11  Planning –  Block World Problem 
### DATE: 22/04/2025                                                                      
### REGISTER NUMBER : 212222040075
### AIM: 
To find the sequence of plan for Block word problem using PDDL  
###  Algorithm:
Step 1 :  Start the program <br>
Step 2 : Create a domain for Block world Problem <br>
Step 3 :  Create a domain by specifying predicates clear, on table, on, arm-empty, holding. <br>
Step 4 : Specify the actions pickup, putdown, stack and un-stack in Block world problem <br>
Step 5 :  In pickup action, Robot arm pick the block on table. Precondition is Block is on table and no other block on specified block and arm-hand empty.<br>
Step 6:  In putdown action, Robot arm place the block on table. Precondition is robot-arm holding the block.<br>
Step 7 : In un-stack action, Robot arm pick the block on some block. Precondition is Block is on another block and no other block on specified block and arm-hand empty.<br>
Step 8 : In stack action, Robot arm place the block on under block. Precondition is Block holded by robot arm and no other block on under block.<br>
Step 9 : Define a problem for block world problem.<br> 
Step 10 : Obtain the plan for given problem.<br> 
     
### Program:
```
ontable(a).
ontable(b).
ontable(c).
clear(a).
clear(b).
clear(c).
handempty.
:- dynamic on/2.
:- dynamic ontable/1.
:- dynamic clear/1.
:- dynamic handempty/0.
:- dynamic holding/1.
pickup(X) :-
    ontable(X),
    clear(X),
    handempty,
    retract(ontable(X)),
    retract(clear(X)),
    retract(handempty),
    assert(holding(X)),
    write('Picked up '), writeln(X).

putdown(X) :-
    holding(X),
    retract(holding(X)),
    assert(ontable(X)),
    assert(clear(X)),
    assert(handempty),
    write('Put down '), writeln(X).

unstack(X, Y) :-
    on(X, Y),
    clear(X),
    handempty,
    retract(on(X, Y)),
    retract(clear(X)),
    retract(handempty),
    assert(holding(X)),
    assert(clear(Y)),
    write('Unstacked '), write(X), write(' from '), writeln(Y).

stack(X, Y) :-
    holding(X),
    clear(Y),
    retract(holding(X)),
    retract(clear(Y)),
    assert(on(X, Y)),
    assert(clear(X)),
    assert(handempty),
    write('Stacked '), write(X), write(' on '), writeln(Y).

goal_plan :-
    pickup(b),
    stack(b, c),
    pickup(a),
    stack(a, b).
```

### Output:

![image](https://github.com/user-attachments/assets/b4e5748d-831c-42bf-96df-529e11f76fe8)

### Result:
Thus the plan was found for the initial and goal state of block world problem.
