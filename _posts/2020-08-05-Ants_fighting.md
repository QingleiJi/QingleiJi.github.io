---
title: "Ants fighting!"
description: Ants seems to be a marvellous species. They act as one, in thousands and millions.
date: 2020-08-05
layout: post
---
When I walked to the campus this morning, I listened to a talk introducing ants. One feature of ants that interests me most is how the ants decide to battle or ceasefire during a fight between two camps. As ants is actually a 2D animal, it's not possible for them to judge the situation of the battle from a higher view. Thus, how they decide to move forward or backward, as explained by the speaker, is actually by counting the numbers of enemies and friends around each individual and compare the numbers to make strategies.

This motivates me to do some simulation...

Lets assume that the two groups of ants (blue ants and red ants) move in a square area. They have home on two sides of the area. One day, due to some unknown reasons, they decided to organize a group fight. The target is to get to the home of their counterpart. However, they are not stupid. According to the speaker, each individual will observe their surroundings and decide where to go. We assume that if in one ant's sight, it found more friends than eenemies then it goes towards the enemies's house by one step. Otherwise it will move backward towards its home. During the 'fight', no ants dies and they can crowd into one place as much as they want. What they care is only to get to another home!

<img src="https://byl5ca.db.files.1drv.com/y4mGDEaZ0kRT1u3SZuXqiOoo2OrOwgZfKWDfkWzlTojdVfZP0tjHzz8TvhEjXdQv9CYqaIUxgtW-b2siNNB2bUgf_bdEc2EcE0zpLaqXPj0ViXam95uU2q9Y9vB4lPp2i7OqfC-qp9ce15bkes7XcJfA8ZwxMZPr5tIQPNCu2ya0V0YHQGBBaw6Usi4_Y02AF7OlVAqo6MCRHhrc_mmTwqGqg?width=1024&height=459&cropmode=none" width="1024" height="459" />

Well, first, we need to abstract the battle to a matlab plot... Here it is with random ants distribution and equal number of ants. The battle field is divided to $$30 \times 30$$ grids and ants are initially distributed randomly and uniformly on the field ($$epoch = 1$$).
<img src="https://dcl5ca.db.files.1drv.com/y4mOBfuW5EvNJ_yPqqxUbab3oYppWd2Qh4EFFQgVkJHT5FiD-3cR3HWFvkGY4fEL-kWE0_ebD9k6ABGMZES53nOP5gRahOxVwCk_ctmvBf_Hs3FETBJxuWAnez-2eun25dLKfMF8hJJgEjZodeuKPfuutikZJ4ojb3tyyz3_Mp10hGIY6zH7WsB2OGkSipX0yarQqoGgwAhm3aC_rCx6sMyqw?width=1024&height=1024&cropmode=none" width="600" height="600" />

Then, war starts.

<img src="/images/2020-08-05-Ants_fighting/equal_ants_10_epoch_animation.gif" width="600" height="600" />

Red army pushes towards the blue home on the top while some red individual that are surrounded by blues have to move back to gather with their mates. Well, for the blue, same case. As it is a fair war from the beginning. The war finally ends after $$31$$ rounds of fight. The ants changed their homes and I guess they are all happy now. Emm, maybe not those who are forced to stay at their original house by their enemy...
<img src="https://dcltca.db.files.1drv.com/y4m_GO6TFYzL48YZTzYPp9ejszzR50kUkCPvC3CsLxSOEH3uePC4ECvzTKtyxy41vRNwYmSdukaE6Q-ZvyV-qBaTTeV_vNCtPBvSxnGhHkqDPmSgEQRaWVOvH0Tw1y_ixKUmJ3JDIKiIuNTwPYAHZeeZlJvui6aZZmy-IUwXj9RaoMG3mSyq9MnOT2h-6fFMkwQpTz8gGX_2TDrEEtqEwy7rg?width=800&height=800&cropmode=none" width="600" height="600" />

What if, it is not a fair war... Let's watch!
<img src="/images/2020-08-05-Ants_fighting/red_majority_entire_battle.gif" width="600" height="600" />

Such a easy game for the reds! In the case, most of the blues have to stay at their home helplessly while only some lucky guys can share the empty house of the reds.

There are still a lot to play later. For instance:
- The ants can move in both $$x$$ and $$y$$ directions;
- The target might not be so simple as now, what about wipe out the enemies?
- What if the ants move to their peloton instead of going towards their home? Will there be cluster in this circumstance and what would it be like?

Emm, more game to play then. 

### Appendix code

```python
import numpy as np
import random
import matplotlib.pyplot as plt


def initialize_ant_info(ant_number, ant_info_number, ant_distribution_ratio, playfield_xlim, playfield_ylim):
    ant_info = np.zeros([ant_number, ant_info_number])
    for i in range(ant_number):
        ant_info[i,0] = i
        if random.uniform(0, 1) <= ant_distribution_ratio:
            ant_info[i,1] = 0
        else:
            ant_info[i,1] = 1
        ant_info[i,2] = random.randint(0, playfield_xlim)
        ant_info[i,3] = random.randint(0, playfield_ylim)
    return ant_info


def initialize_ant_action(ant_number, ant_info_number, ant_info):
    ant_action = np.zeros([ant_number, ant_info_number])
    ant_action[:, 0:2] = ant_info[:, 0:2]
    return ant_action


def ant_action_updatter(ant_number, ant_info, ant_action, playfield_ylim):
    for ant_i in range(ant_number):
        friend_number = 0
        enemy_number = 0
        for ant_j in range(ant_number):
            ant_distance = np.sqrt((ant_info[ant_i, 2]-ant_info[ant_j, 2])**2+(ant_info[ant_i, 3]-ant_info[ant_j, 3])**2)
            ant_detect_distance = 2
            if ant_info[ant_i, 1] == ant_info[ant_j, 1] and ant_distance <= ant_detect_distance:
                friend_number += 1
            elif ant_info[ant_i, 1] != ant_info[ant_j, 1] and ant_distance <= ant_detect_distance:
                enemy_number += 1
            if (friend_number >= enemy_number and ant_info[ant_i, 1] == 0 and ant_info[ant_i, 3] < playfield_ylim) or (friend_number < enemy_number and ant_info[ant_i, 1] == 1 and ant_info[ant_i, 3] < playfield_ylim):
                ant_action[ant_i, 3] = 1
            elif (friend_number < enemy_number and ant_info[ant_i, 1] == 0 and ant_info[ant_i, 3] > 0) or (friend_number >= enemy_number and ant_info[ant_i, 1] == 1 and ant_info[ant_i, 3] > 0):
                ant_action[ant_i, 3] = -1
            else:
                ant_action[ant_i, 3] = 0
    return ant_action

def ant_info_updatter(ant_info, ant_action):
    ant_info[:, 3] = ant_info[:, 3] + ant_action[:, 3]
    return ant_info

def main():
#    ---------------
#   |  ↓ 1, BLUE ↓  |
#   |  ↓         ↓  |
#   |               |
#   |  ↑         ↑  |
#   |  ↑ 0, RED  ↑  |
#    ---------------

    ant_number = 100
    ant_info_number = 4  # number, identity, position_x, position_y
    playfield_xlim = 30
    playfield_ylim = 30
    ant_distribution_ratio = 0.5
    ant_info = initialize_ant_info(ant_number, ant_info_number, ant_distribution_ratio, playfield_xlim, playfield_ylim)
    ant_action = initialize_ant_action(ant_number, ant_info_number, ant_info)

    epoch = 10

    plt.figure(figsize=(8, 8), dpi=100)
    plt.ion()
    for epo in range(epoch):
        ant_action = ant_action_updatter(ant_number, ant_info, ant_action, playfield_ylim)
        if (ant_action[:, 3] == 0).all(): # break if no change any more
            break
        ant_info = ant_info_updatter(ant_info, ant_action)
        epo += 1
        
        plt.cla()
        plt.title("Ants fighting! Epoch: " + str(epo))
        plt.grid(True)
        plt.xticks(np.arange(0, playfield_xlim+1, step=1))
        plt.yticks(np.arange(0, playfield_ylim + 1, step=1))

        for i in range(ant_number):
            if ant_info[i, 1] == 0:
                plt.scatter(ant_info[i, 2], ant_info[i, 3], s=40, c='r', marker='o', alpha=0.9)
            else:
                plt.scatter(ant_info[i, 2], ant_info[i, 3],s=60, c='b', marker='o', alpha=0.9)
        plt.savefig(str(epo), dpi=100)
        plt.pause(0.1)
    plt.ioff()
    plt.show()

if __name__ == '__main__':
    main()

```
