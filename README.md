- 👋 Hi, I’m @snoorzaigithub
- 👀 I’m interested in ...
- 🌱 I’m currently learning ...
- 💞️ I’m looking to collaborate on ...
- 📫 How to reach me ...
- 😄 Pronouns: ...
- ⚡ Fun fact: ...

<!---
snoorzaigithub/snoorzaigithub is a ✨ special ✨ repository because its `README.md` (this file) appears on your GitHub profile.
You can click the Preview link to take a look at your changes.
--->
#!/usr/bin/python3

import sys
from collections import deque

# if we want to parse the map from a given filename
def parse_map(filename):
    with open(filename, "r") as f:
        return [list(line.strip()) for line in f.readlines()][3:]

# if we want to row, col index pair is on the map and valid
def is_valid(pos, n, m, castle_map):
    row, col = pos
    return 0 <= row < n and 0 <= col < m and castle_map[row][col] in ".@"

# while we need to get possible moves from a given position
def get_moves(map, row, col):
    directions = [(1, 0), (-1, 0), (0, -1), (0, 1)]  # Down, Up, Left, Right
    return [(row + dr, col + dc) for dr, dc in directions if is_valid((row + dr, col + dc), len(map), len(map[0]), map)]

# when we want to get direction (U, D, L, R) between two positions
def get_direction(from_pos, to_pos):
    row1, col1 = from_pos
    row2, col2 = to_pos
    if row2 == row1 + 1: return "D"
    if row2 == row1 - 1: return "U"
    if col2 == col1 + 1: return "R"
    if col2 == col1 - 1: return "L"

def search(castle_map):
    start = [(row, col) for row in range(len(castle_map)) for col in range(len(castle_map[0])) if castle_map[row][col] == "p"][0]
    
    queue = deque([(start, 0, "")])  # Queue to store (position, distance, path)
    visited = set([start])           # Set to track visited positions

    while queue:
        (current_pos, distance, path) = queue.popleft()  # Pop from left for BFS
        
        for move in get_moves(castle_map, *current_pos):
            if castle_map[move[0]][move[1]] == "@":  # Check if portal is found
                  return distance + 1, path + get_direction(current_pos, move)
            
            if move not in visited:  # Add unvisited valid moves
                 visited.add(move)
                  queue.append((move, distance + 1, path + get_direction(current_pos, move)))

     return -1, ""  # If no path is found

# and finally here is Main function to run the program
 if __name__ == "__main__":
    castle_map = parse_map(sys.argv[1])
     print("Shhhh... quiet while I navigate!")
     move_count, move_path = search(castle_map)
    print(f"Here's the solution I found:\n{move_count} {move_path}")  
