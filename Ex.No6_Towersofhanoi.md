# Ex.No: 6   Logic Programming – Factorial of number   
### DATE: 08/04/2025                                                                    
### REGISTER NUMBER : 212222040075
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
hanoi(1, Source, Target, _, [move(Source, Target)]) :- !.
hanoi(N, Source, Target, Auxiliary, Moves) :-
    N > 1,
    N1 is N - 1,
    hanoi(N1, Source, Auxiliary, Target, Moves1),
    hanoi(1, Source, Target, _, Move2),
    hanoi(N1, Auxiliary, Target, Source, Moves3),
    append(Moves1, Move2, TempMoves),
    append(TempMoves, Moves3, Moves).

print_moves([]).
print_moves([move(From, To)|Rest]) :-
    format('Move disk from ~w to ~w~n', [From, To]),
    print_moves(Rest).

solve_hanoi(N) :-
    hanoi(N, left, right, center, Moves),
    print_moves(Moves).

```
### Output:

![image](https://github.com/user-attachments/assets/e3b1dc2c-52b9-4d14-85bd-3ea0a477f4f8)

### Result:
Thus the solution of Towers of Hanoi problem was found by logic programming.
