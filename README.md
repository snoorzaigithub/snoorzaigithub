#!/usr/local/descktop/python3
# mystical_castle.py 
# Submitted by : Susan Noorzai/snoorzaigithub
# CSCI B551, Fall 2024. 
# Compeuter Scince MS

import sys
from collections import deque

def parse_map(filename):
    #if we want to parse the map from the given file and return it as a 2D list.
    with open(filename, "r") as f:
        return [[char for char in line.strip()] for line in f]

def valid_index(pos, n, m):
    #Check if the position is within the map boundaries.
    return 0 <= pos[0] < n and 0 <= pos[1] < m

def moves(map, row, col):
    #Get valid moves from the current position.
    possible_moves = ((row + 1, col), (row - 1, col), (row, col - 1), (row, col + 1))
    return [move for move in possible_moves if valid_index(move, len(map), len(map[0])) and map[move[0]][move[1]] in ".@"]

def get_direction(from_pos, to_pos):
    #Determine the direction of movement from one position to another.
    if from_pos[0] == to_pos[0] - 1:
        return "D"         # shift to down
    if from_pos[0] == to_pos[0] + 1:
        return "U"             # shift to up
    if from_pos[1] == to_pos[1] - 1:
        return "R"                  # shift to right
    if from_pos[1] == to_pos[1] + 1:
        return "L"                        # shift to left

def search(castle_map):
       # now we want to find the shortest path from start to goal in the castle map.
    rows, cols = len(castle_map), len(castle_map[0])
    start = next((r, c) for r in range(rows) for c in range(cols) if castle_map[r][c] == "p")
    goal = next((r, c) for r in range(rows) for c in range(cols) if castle_map[r][c] == "@")

    queue = deque([(start, 0, "")])      # (position, distance, path)
    visited = set()
    visited.add(start)

    while queue:
        (current_r, current_c), dist, path = queue.popleft()

        for move in moves(castle_map, current_r, current_c):
            if castle_map[move[0]][move[1]] == "@":
                return (dist + 1, path + get_direction((current_r, current_c), move))

            if move not in visited:
                visited.add(move)
                direction = get_direction((current_r, current_c), move)
                queue.append((move, dist + 1, path + direction))

    return (-1, "")  # send back -1 if no path is found

if __name__ == "__main__":
    if len(sys.argv) != 2:
        print("Usage: python3 mystical_castle.py <mapfile>")
        sys.exit(1)

    castle_map = parse_map(sys.argv[1])
    print("please give me a moment to find my way")
    solution = search(castle_map)
    print("done, it's the solution I found:")
    print(f"{solution[0]} {solution[1]}")
