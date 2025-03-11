# Ex.No: 3  Implementation of Minimax Search
### DATE:                                                                            
### REGISTER NUMBER : 
### AIM: 
Write a mini-max search algorithm to find the optimal value of MAX Player from the given graph.
### Algorithm:
1. Start the program
2. import the math package
3. Specify the score value of leaf nodes and find the depth of binary tree from leaf nodes.
4. Define the minimax function
5. If maximum depth is reached then get the score value of leaf node.
6. Max player find the maximum value by calling the minmax function recursively.
7. Min player find the minimum value by calling the minmax function recursively.
8. Call the minimax function  and print the optimum value of Max player.
9. Stop the program. 

### Program:
```
def minimax(depth, index, is_max, values, alpha, beta):
    if depth == 3:
        return values[index]

    func = max if is_max else min
    best = float('-inf') if is_max else float('inf')

    for i in range(2):
        val = minimax(depth + 1, index * 2 + i, not is_max, values, alpha, beta)
        best = func(best, val)

        if is_max:
            alpha = max(alpha, best)
        else:
            beta = min(beta, best)

        if beta <= alpha:
            break

    return best

values = [3, 5, 6, 9, 1, 2, 0, -1]
print("The optimal value is:", minimax(0, 0, True, values, float('-inf'), float('inf')))

```
### Output:

![image](https://github.com/user-attachments/assets/16087860-79ce-4574-9b00-e1c58f49d3bc)

### Result:
Thus the optimum value of max player was found using minimax search.
