# Ex.No: 6   Logic Programming – Factorial of number   
### DATE: 08/04/2025                                                                    
### REGISTER NUMBER : 212222040168
### AIM: 
To  write  a logic program  to solve Towers of Hanoi problem  using SWI-PROLOG. 
### Algorithm:
1. Start the program
2.  Write a rules for finding solution of Towers of Hanoi in SWI-PROLOG.
3.  a )	If only one disk  => Move disk from X to Y.
4.  b)	If Number of disk greater than 0 then
5.        i)	Move  N-1 disks from X to Z.
6.        ii)	Move  Nth disk from X to Y
7.        iii)	Move  N-1 disks from Y to X.
8. Run the program  to find answer of  query.

### Program:
```
move(1, X, Y, _) :-
    format('Move disk from ~w to ~w~n', [X, Y]).
move(N, X, Y, Z) :-
    N > 1,
    N1 is N - 1,
    move(N1, X, Z, Y),
    move(1, X, Y, _),
    move(N1, Z, Y, X).

```
### Output:

![image](https://github.com/user-attachments/assets/b6232674-9355-4a4d-8dc5-30196a98d488)

### Result:
Thus the solution of Towers of Hanoi problem was found by logic programming.
