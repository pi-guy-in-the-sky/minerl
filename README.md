# The [MineRL](http://minerl.io) Python Package

[![Documentation Status](https://readthedocs.org/projects/minerl/badge/?version=latest)](https://minerl.readthedocs.io/en/latest/?badge=latest)
[![Dev Build status](https://badge.buildkite.com/0717cd35b9a708f0b4ac0b2858eec9ca7d08f6768868d3ac08.svg?branch=dev)](https://buildkite.com/openai-mono/minerl-public-dev)
[![Downloads](https://pepy.tech/badge/minerl)](https://pepy.tech/project/minerl)
[![PyPI version](https://badge.fury.io/py/minerl.svg)](https://badge.fury.io/py/minerl)
[!["Open Issues"](https://img.shields.io/github/issues-raw/minerllabs/minerl.svg)](https://github.com/minerllabs/minerl/issues)
[![GitHub issues by-label](https://img.shields.io/github/issues/minerllabs/minerl/bug.svg?color=red)](https://github.com/minerllabs/minerl/issues?utf8=%E2%9C%93&q=is%3Aissue+is%3Aopen+label%3Abug)
[![Discord](https://img.shields.io/discord/565639094860775436.svg?label=&logo=discord&logoColor=ffffff&color=7389D8&labelColor=6A7EC2)](https://discord.gg/BT9uegr)


Python package providing easy to use gym environments and a simple data api for the MineRLv0 dataset. 

**To [get started please read the docs here](http://minerl.io/docs/)!**

![](http://www.minerl.io/docs/_images/demo.gif)
## Installation

With JDK-8 installed run this command
```
pip3 install --upgrade minerl
```

## Basic Usage

Running an environment:
```python
import minerl
import gym
env = gym.make('MineRLNavigateDense-v0')


obs = env.reset()

done = False
while not done:
    action = env.action_space.sample() 
 
    # One can also take a no_op action with
    # action =env.action_space.noop()
    
 
    obs, reward, done, info = env.step(
        action)

```

Sampling the dataset:

```python
import minerl

# YOU ONLY NEED TO DO THIS ONCE!
minerl.data.download('/your/local/path')

data = minerl.data.make(
    'MineRLObtainDiamond-v0',
    data_dir='/your/local/path')

# Iterate through a single epoch gathering sequences of at most 32 steps
for current_state, action, reward, next_state, done \
    in data.batch_iter(
        num_epochs=1, seq_len=32):

        # Print the POV @ the first step of the sequence
        print(current_state['pov'][0])

        # Print the final reward pf the sequence!
        print(reward[-1])

        # Check if final (next_state) is terminal.
        print(done[-1])

        # ... do something with the data.
        print("At the end of trajectories the length"
              "can be < max_sequence_len", len(reward))
```


Visualizing the dataset:

![viewer|540x272](http://www.minerl.io/docs/_images/cropped_viewer.gif)
```bash

# Make sure your MINERL_DATA_ROOT is set!
export MINERL_DATA_ROOT='/your/local/path'

# Visualizes a random trajectory of MineRLObtainDiamondDense-v0
python3 -m minerl.viewer MineRLObtainDiamondDense-v0

```

## MineRL Competition
If you're here for the MineRL competition. Please check [the main competition website here](https://www.aicrowd.com/challenges/neurips-2021-minerl-competition).
