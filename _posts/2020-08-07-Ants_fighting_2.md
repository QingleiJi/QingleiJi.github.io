---
title: "Ants fighting! Round 2."
description: Follwing the battle the other day, the same groups of ants decide to change the battle strategy this time.
date: 2020-08-07
layout: post
---
Last time, the two groups of ants had a very simple [fight](https://qinglei.tech/posts/ants_fighting/). They both occupied the enemies' home at last. Well, now, after some careful investigation, they upgrated the battle policy. On the same $$30 \times 30$$ battle field, the same $$100$$ ants now follows new rules:
- Each ant look around and counts the ememies and friends within a detection range.
- Each ant compares the number of enemies and friends, if more friends are around, it will analyse the distribution of the enemies and find out which direction ($$+x,-x,+y,-y$$) is the weakness of the enemy (least number of enemies). Then it will move towards that direction for $$1$$ step to have a fight. On the other hand, if less friend is around, it will go to the direction where more friends are there.
- If equal friends and enemies are in sight, the ant will choose to stay in the same position to keep observation.

OK. First round, we try a detection range with $$5$$, meaning that each ant can only sees a small field.

<img src="/images/2020-08-07-Ants_fighting_2/detection distance 5.gif" width="600" height="600" />

From the animation, we can see that small clusters of the same group of ants can be observed. However, the ants are not showing very obvious tendency to fight as a team. What about increasing the detection distance of each ant to $$10$$? 

<img src="/images/2020-08-07-Ants_fighting_2/detection distance 10.gif" width="600" height="600" />

This time, ants of a larger scale decides to move into several rallying points. Also, some unique ants are too far away from their people and do not know where to go. What if the ants can see further to $$15$$?

<img src="/images/2020-08-07-Ants_fighting_2/detection distance 15.gif" width="600" height="600" />

Now, it is very obvious that almost all the the ants assembles quickly to the center. The distribution of the two groups looks like two perpendicular stripes. I guess they all prefer to avoid the enemies and fall to the embrace of the friends.

Well, that's it for today. An interesting game for me, maybe for the ants as well...

### Appendix code

```python
import numpy as np
import random
import matplotlib.pyplot as plt


def initialize_ant_info(ant_number, ant_info_number, ant_distribution_ratio, playfield_xlim, playfield_ylim):
    ant_info = np.zeros([ant_number, ant_info_number])
    for i in range(ant_number):
        ant_info[i, 0] = i
        if random.uniform(0, 1) <= ant_distribution_ratio:
            ant_info[i, 1] = 0
        else:
            ant_info[i, 1] = 1
        ant_info[i, 2] = random.randint(0, playfield_xlim)
        ant_info[i, 3] = random.randint(0, playfield_ylim)
    return ant_info


def initialize_ant_action(ant_number, ant_info_number, ant_info):
    ant_action = np.zeros([ant_number, ant_info_number])
    ant_action[:, 0:2] = ant_info[:, 0:2]
    return ant_action


def ant_action_updatter(ant_number, ant_info, ant_action, playfield_ylim):
    for ant_i in range(ant_number):
        friend_number = 0
        enemy_number = 0
        friend_sum_position_x = 0
        friend_sum_position_y = 0
        enemy_sum_position_x = 0
        enemy_sum_position_y = 0
        for ant_j in range(ant_number):
            ant_distance = np.sqrt(
                (ant_info[ant_i, 2]-ant_info[ant_j, 2])**2+(ant_info[ant_i, 3]-ant_info[ant_j, 3])**2)
            ant_detect_distance = 25
            if ant_i != ant_j:
                if ant_info[ant_i, 1] == ant_info[ant_j, 1] and ant_distance <= ant_detect_distance:
                    friend_number += 1
                    friend_sum_position_x += ant_info[ant_j, 2]
                    friend_sum_position_y += ant_info[ant_j, 3]
                elif ant_info[ant_i, 1] != ant_info[ant_j, 1] and ant_distance <= ant_detect_distance:
                    enemy_number += 1
                    enemy_sum_position_x += ant_info[ant_j, 2]
                    enemy_sum_position_y += ant_info[ant_j, 3]
        if friend_number == 0 and enemy_number == 0:
            ant_action[ant_i, 2] = 0
            ant_action[ant_i, 3] = 0
            #ant_action[ant_i, 3] = random.randint(-1,1)
        elif friend_number == 0 and enemy_number != 0:
            enemy_average_position_x = enemy_sum_position_x/enemy_number
            enemy_average_position_y = enemy_sum_position_y/enemy_number
            if enemy_average_position_x <= ant_info[ant_i, 2]:
                enemy_position_direction_x = -1
            else:
                enemy_position_direction_x = 1
            if enemy_average_position_y <= ant_info[ant_i, 3]:
                enemy_position_direction_y = -1
            else:
                enemy_position_direction_y = 1
            if enemy_average_position_x <= enemy_average_position_y:
                ant_action[ant_i, 3] = -1*enemy_position_direction_y
                ant_action[ant_i, 2] = 0
            else:
                ant_action[ant_i, 2] = -1*enemy_position_direction_x
                ant_action[ant_i, 3] = 0
        elif friend_number != 0 and enemy_number == 0:
            friend_average_position_x = friend_sum_position_x/friend_number
            friend_average_position_y = friend_sum_position_y/friend_number
            if friend_average_position_x <= ant_info[ant_i, 2]:
                friend_position_direction_x = -1
            else:
                friend_position_direction_x = 1
            if friend_average_position_y <= ant_info[ant_i, 3]:
                friend_position_direction_y = -1
            else:
                friend_position_direction_y = 1
            if friend_average_position_x <= friend_average_position_y:
                ant_action[ant_i, 3] = -1*friend_position_direction_y
                ant_action[ant_i, 2] = 0
            else:
                ant_action[ant_i, 2] = -1*friend_position_direction_x
                ant_action[ant_i, 3] = 0
        else:
            friend_average_position_x = friend_sum_position_x/friend_number
            friend_average_position_y = friend_sum_position_y/friend_number
            enemy_average_position_x = enemy_sum_position_x/enemy_number
            enemy_average_position_y = enemy_sum_position_y/enemy_number

            if friend_average_position_x <= ant_info[ant_i, 2]:
                friend_position_direction_x = -1
            else:
                friend_position_direction_x = 1
            if friend_average_position_y <= ant_info[ant_i, 3]:
                friend_position_direction_y = -1
            else:
                friend_position_direction_y = 1
            if enemy_average_position_x <= ant_info[ant_i, 2]:
                enemy_position_direction_x = -1
            else:
                enemy_position_direction_x = 1
            if enemy_average_position_y <= ant_info[ant_i, 3]:
                enemy_position_direction_y = -1
            else:
                enemy_position_direction_y = 1

            if friend_number > enemy_number:
                if enemy_average_position_x <= enemy_average_position_y:
                    ant_action[ant_i, 2] = 1*enemy_position_direction_x
                    ant_action[ant_i, 3] = 0
                else:
                    ant_action[ant_i, 3] = 1*enemy_position_direction_y
                    ant_action[ant_i, 2] = 0
            elif friend_number < enemy_number:
                if friend_average_position_x <= friend_average_position_y:
                    ant_action[ant_i, 3] = 1*friend_position_direction_y
                    ant_action[ant_i, 2] = 0
                else:
                    ant_action[ant_i, 2] = 1*enemy_position_direction_x
                    ant_action[ant_i, 3] = 0
            else:
                ant_action[ant_i, 2] = 0
                ant_action[ant_i, 3] = 0
    return ant_action


def ant_info_updatter(ant_info, ant_action, playfield_xlim, playfield_ylim):
    ant_info[:, 2] = ant_info[:, 2] + ant_action[:, 2]
    ant_info[:, 3] = ant_info[:, 3] + ant_action[:, 3]
    for i in range(ant_info.shape[0]):
        if ant_info[i, 2] < 0:
            ant_info[i, 2] = 0
        elif ant_info[i, 2] > playfield_xlim:
            ant_info[i, 2] = playfield_xlim
        if ant_info[i, 3] < 0:
            ant_info[i, 3] = 0
        elif ant_info[i, 3] > playfield_ylim:
            ant_info[i, 3] = playfield_ylim
    return ant_info


def main():

    ant_number = 100
    ant_info_number = 4  # number, identity, position_x, position_y
    playfield_xlim = 30
    playfield_ylim = 30
    ant_distribution_ratio = 0.5
    ant_info = initialize_ant_info(
        ant_number, ant_info_number, ant_distribution_ratio, playfield_xlim, playfield_ylim)
    ant_action = initialize_ant_action(ant_number, ant_info_number, ant_info)

    epoch = 60

    plt.figure(figsize=(8, 8), dpi=100)
    plt.ion()
    for epo in range(epoch):
        ant_action = ant_action_updatter(
            ant_number, ant_info, ant_action, playfield_ylim)
        if (ant_action[:, 3] == 0).all():  # break if no change any more
            break
        ant_info = ant_info_updatter(
            ant_info, ant_action, playfield_xlim, playfield_ylim)

        epo += 1

        plt.cla()
        plt.title("Ants fighting! Epoch: " + str(epo))
        plt.grid(True)
        plt.xlim(-.5, playfield_xlim + .5)
        plt.ylim(-.5, playfield_ylim + .5)
        plt.xticks(np.arange(0, playfield_xlim + 1, step=1))
        plt.yticks(np.arange(0, playfield_ylim + 1, step=1))

        for ant in range(ant_number):
            if ant_info[ant, 1] == 0:
                plt.scatter(ant_info[ant, 2], ant_info[ant, 3],
                            s=40, c='r', marker='o', alpha=0.9)
            else:
                plt.scatter(ant_info[ant, 2], ant_info[ant, 3],
                            s=60, c='b', marker='o', alpha=0.9)
        plt.savefig(str(epo), dpi=100)
        plt.pause(0.1)
    plt.ioff()
    plt.show()


if __name__ == '__main__':
    main()
```
