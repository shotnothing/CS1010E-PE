## Guide: General Structure of a BFS

```python
def bfs(initial_state): 
    visited = set()
    # put in the initial state into the queue
    queue = [initial_state]
    
    while queue:
        # get next state and delete it from the queue (thats what pop means)
        current_state = queue.pop(0)
        
        # skip if visited this state before
        if current_state in visited:
            continue
        else:
            visited.add(current_state)
         
        # terminate early if its a winning state   
        if check_win(current_state):
            return current_state
            
        # else add the next possible states to the queue
        queue.extend(successors(current_state))
        
def check_win(state):
    # ... specify yourself based on game details
    # returns True if win, else False
    
def successors(state):
    # ... specify yourself based on game details
    # returns a list of valid next states
```


## Sudoku Challenge:

```python
''' Solve Sudoku (BFS Practice, hard)
by jing wen

I wrote 2 functions for you! 
- generate_board() : generates a valid sudoku board which is a list of list of strings
- print_board(board) : prints the board given in the input argument

In the board, '_' represents empty space where you put your guesses.
Write a BFS to solve the board.
'''

import random
def generate_board(): # don't need to know how this works
    a = [[[str(tuple(range(1,10))[(i*3 + i//3 + j) % 9]) if random.random()>0.2 else '_' for j in range(9)] for i in range(9)][i*3:i*3+3][j] for i in range(3) for j in random.sample([0, 1, 2], 3)].copy()
    b = [[[a[j][i] for j in range(9)] for i in range(9)][i*3:i*3+3][j] for i in range(3) for j in random.sample([0, 1, 2], 3)]
    return b
print_board = lambda board: print('\n'.join(' '.join(board[i][j] for i in range(9))for j in range(9))) # don't need to know how this works

game = generate_board()
print_board(game) 

# ------------------------------------------------------------
# Write your bfs to solve board under here  (may have multiple possible solutions, any will do)
def bfs(board):
    pass

print() # newline
print_board(bfs(game))
```

possible solution: https://pastebin.com/cCy54RDx
