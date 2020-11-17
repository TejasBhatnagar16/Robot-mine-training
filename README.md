# robot-train-mine-
# A simple problem wherein I had to find the total time spent by all the robots preforming  their tasks. The solution is written in python and utilizes a prefix sum array to make it fast enough. It was tested with a maximum of 100,000 commands and took around 0.6 seconds to evaluate the results.  
import time
import itertools


def move_and_add_time(start, direction, distance, b, c):
    # inputs start and end position and position of other two robos
    # returns added time
    if direction == '1':
        end = min((start + distance - 1), len(times) - 1)
    else:
        end = max((start - distance + 1), 0)
    added_time = 0
    start, end_pos = sorted([start, end])
    if start == 0:
        added_time += prefix_sum[end_pos]
    else:
        added_time = abs(prefix_sum[start - 1] - prefix_sum[end_pos])
    lower_b = max((b - rad), 0)
    upper_b = min((b + rad), len(times) - 1)
    lower_c = max((c - rad), 0)
    upper_c = min((c + rad), len(times) - 1)
    intersection1 = check_intersection(start, end_pos, lower_b, upper_b)
    intersection2 = check_intersection(start, end_pos, lower_c, upper_c)
    if intersection2 and intersection1:
        intersection3 = check_intersection(
            intersection1[0], intersection1[1], intersection2[0], intersection2[1])
    else:
        intersection3 = None
    if intersection1:
        if intersection1[0] == 0:
            added_time += prefix_sum[intersection1[1]]
        else:
            added_time += prefix_sum[intersection1[1]
                                     ] - prefix_sum[intersection1[0] - 1]
    if intersection2:
        if intersection2[0] == 0:
            added_time += prefix_sum[intersection2[1]]
        else:
            added_time += prefix_sum[intersection2[1]
                                     ] - prefix_sum[intersection2[0] - 1]
    if intersection3:
        if intersection3[0] == 0:
            added_time -= prefix_sum[intersection3[1]]
        else:
            added_time -= prefix_sum[intersection3[1]
                                     ] - prefix_sum[intersection3[0] - 1]
    return end, added_time


def check_intersection(start, end, b, c):
    if end < b or c < start:
        return None
    else:
        return sorted([max(start, b), min(end, c)])


def solver():
    # sorts the data and calls move and add time
    total_time = 0
    line1 = input('line1: ')
    global rad, times, prefix_sum
    length, rad, x, y, z = line1.split()
    length, rad, x, y, z = int(length), int(rad), int(x), int(y), int(z)
    line2 = input('line 2: ')
    times = line2.split()
    times = list(map(int, times))
    prefix_sum = list(itertools.accumulate(times))
    line3 = input('line3: ')
    for j in range(int(line3)):
        moves = input('moves: ')
        robo, dire, dist = moves.split()
        if robo == '1':
            x, added_time = move_and_add_time(x, dire, int(dist), y, z)
            total_time += added_time
        elif robo == '2':
            y, added_time = move_and_add_time(y, dire, int(dist), x, z)
            total_time += added_time
        else:
            z, added_time = move_and_add_time(z, dire, int(dist), y, x)
            total_time += added_time
    return total_time


print(solver())
