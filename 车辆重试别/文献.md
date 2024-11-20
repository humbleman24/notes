## Swarm intelligence

swarm intelligence is based on the idea that autonomous agents can solve complex tasks together by following simple rules.

it is decentralized: agent decide their actions independently based on their interaction with the enviornment (no predefined leader)

**Basic principles:**

- the swarm can solve complex problems that a single individual with simple abilities could not solve
- the performance of the swarm is not affected by loss of one individual or a mistake by one individual
- individuals have local sensory information and thy can perform simple actions, but they have little or no memory, and they do not know about the status of the swarm as a whole or its goal

### Particle swarm optimization (PSO)

goal: find the area of the highest concentration of insects

background settings: the birds can signalize their position and the food concentration there to their neighbors; birds can remember their past position of the best food concentration.

##### programming 

**Each particle $i$** is described by its current position $x_i$, best known position $p_i$, velocity $v_i$, and cost function values  $f(x_i)$ and $f(p_i)$. 

**Goal**: we want to find the position that minimizes the cost function 

**Initial settings:** positions and velocities of the particles are initialized randomly, initialize  $p_i$ is set to $x_i$; the swarms best known position $g$ is set to the $x_i$ that minimizes $f(g)$.

