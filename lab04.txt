from Stack import Stack


def printMaze(maze):
    for row in range(len(maze)):
        for col in range(len(maze[0])):
            print("|{:<2}".format(maze[row][col]), sep='', end='')
        print("|")
    return


def solveMaze(maze, startX, startY):
    maze[startX][startY] = 1
    s = Stack()
    s.push((startX, startY))

    def singleSpace():
        ini_length = s.size()
        x = s.peek()[0]
        y = s.peek()[1]
        x_max = len(maze[0]) - 1
        y_max = len(maze) - 1

        for (i, j) in [(-1, 0), (0, 1), (1, 0), (0, -1)]:
            if 0 <= x+i <= x_max+1 and 0 <= y+j <= y_max+1:
                s.push((x + i, y + j))
                try:
                    if maze[s.peek()[0]][s.peek()[1]] == " ":
                        maze[s.peek()[0]][s.peek()[1]] = s.size()
                        break
                    elif maze[s.peek()[0]][s.peek()[1]] == "G":
                        s.pop()
                        return False
                    else:
                        s.pop()
                except IndexError:
                    s.pop()

        end_length = s.size()
        if ini_length < end_length:
            return True
        elif ini_length == end_length:
            return None

    not_reach_goal = True
    while not_reach_goal:
        not_reach_goal = singleSpace()
        if not_reach_goal is None:
            s.pop()
            s.push(s.peek())
            not_reach_goal = singleSpace()
            if not_reach_goal is None:
                return False
            else:
                continue

    return True
