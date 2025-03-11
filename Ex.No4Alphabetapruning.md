# Ex.No: 4   Implementation of Alpha Beta Pruning 
### DATE: 11/03/2025                                                            
### REGISTER NUMBER : 212222040075
### AIM: 
Write a Alpha beta pruning algorithm to find the optimal value of MAX Player from the given graph.
### Steps:
1. Start the program
2. Initially  assign MAX and MIN value as 1000 and -1000.
3.  Define the minimax function  using alpha beta pruning
4.  If maximum depth is reached then return the score value of leaf node. [depth taken as 3]
5.  In Max player turn, assign the alpha value by finding the maximum value by calling the minmax function recursively.
6.  In Min player turn, assign beta value by finding the minimum value by calling the minmax function recursively.
7.  Specify the score value of leaf nodes and Call the minimax function.
8.  Print the best value of Max player.
9.  Stop the program. 

### Program:
```
import math
def alpha_beta_pruning(depth, node_index, is_maximizing, values, alpha, beta):
    if depth == 3:
        return values[node_index]
    
    if is_maximizing:
        best = -math.inf
        for i in range(2):
            val = alpha_beta_pruning(depth + 1, node_index * 2 + i, False, values, alpha, beta)
            best = max(best, val)
            alpha = max(alpha, best)
            if beta <= alpha:
                break
        
        return best
    else:
        best = math.inf
        
        for i in range(2):
            val = alpha_beta_pruning(depth + 1, node_index * 2 + i, True, values, alpha, beta)
            best = min(best, val)
            beta = min(beta, best)
            if beta <= alpha:
                break

        return best
    
values = [3, 5, 6, 9, 1, 2, 0, -1]  
result = alpha_beta_pruning(0, 0, True, values, -math.inf, math.inf)
print("The Optimal value is: ", result)
```
### Output:

![image](https://github.com/user-attachments/assets/f293a5ea-3391-45e3-80d0-6586e29cab3d)

### Result:
Thus the best score of max player was found using Alpha Beta Pruning.
