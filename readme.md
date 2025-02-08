# My implementation of Openai Spinningup

For installation, following instructions at [spinninup website](https://spinningup.openai.com/en/latest/user/algorithms.html). Note that I've changed some dependencies, such as specify `opencv-python==3.4.13.47` to make the installation successful now.

To suppoert MUJOCO envs is a bit tedious, you need to:
1. Go to the [mujoco-py](https://github.com/openai/mujoco-py) github page. Follow the installation instructions in the README
2. Then, install some libraries (assume you are using Ubuntu)
   ```sh
   sudo apt install libosmesa6-dev libgl1-mesa-glx libglfw3
   sudo apt-get install patchelf # From https://gist.github.com/saratrajput/60b1310fe9d9df664f9983b38b50d5da
   pip install "cython<3" # From https://github.com/openai/mujoco-py/issues/773#issuecomment-1639684035
   sudo apt-get install libglew-dev # From https://stackoverflow.com/questions/76985054/import-mujoco-py-is-giving-me-compiling-errors
   ```
3. To install mujoco-py:
   ```sh
   wget https://mujoco.org/download/mujoco210-linux-x86_64.tar.gz
   mkdir -p ~/.mujoco
   tar -xvzf mujoco210-linux-x86_64.tar.gz -C ~/.mujoco
   ls ~/.mujoco/mujoco210
   pip3 install -U 'mujoco-py<2.2,>=2.1'
   ```
4. Setup some env variables:
   ```sh
   export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/home/lyk/.mujoco/mujoco210/bin
   export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/usr/lib/nvidia
   ```
   

After that, you should be able to run this script:
```python
import mujoco_py
import os
mj_path = mujoco_py.utils.discover_mujoco()
xml_path = os.path.join(mj_path, 'model', 'humanoid.xml')
model = mujoco_py.load_model_from_path(xml_path)
sim = mujoco_py.MjSim(model)

print(sim.data.qpos)
# [0. 0. 0. 0. 0. 0. 0. 0. 0. 0. 0. 0. 0. 0. 0. 0. 0. 0. 0. 0. 0.]

sim.step()
print(sim.data.qpos)
# [-2.09531783e-19  2.72130735e-05  6.14480786e-22 -3.45474715e-06
#   7.42993721e-06 -1.40711141e-04 -3.04253586e-04 -2.07559344e-04
#   8.50646247e-05 -3.45474715e-06  7.42993721e-06 -1.40711141e-04
#  -3.04253586e-04 -2.07559344e-04 -8.50646247e-05  1.11317030e-04
#  -7.03465386e-05 -2.22862221e-05 -1.11317030e-04  7.03465386e-05
#  -2.22862221e-05]
```

Afterwards, you can run algos in mujoco envs. For instance,
```sh
python -m spinup.run sac --env Walker2d-v2 --exp_name walker 
```


This project uses Python=3.6, VSCode python debugging [doesn't support](https://marketplace.visualstudio.com/items?itemName=ms-python.debugpy) this version.  You have to install an older version of the main VSCode Python extensions.

I had to install the following VSCode extensions at these versions:

    Python v2022.8.1

    Pylance v2022.6.30

    Python Debugger v2023.1.XXX (pre-release version | debugpy v1.5.1)

Note: 
* To watch the game-play video, `env.render()` will be called, which levereges pyglet, which needs a screen connected to your machine. For some reason, VNC does not tackle this requirement. You can see https://stackoverflow.com/questions/40195740/how-to-run-openai-gym-render-over-a-server  for solutions, but none of them is quite satisfying yet.
* Curretnly spiningup doesn't support GPU, and I failed to add this support. The reason is unknow, see my github [issue](https://github.com/openai/spinningup/issues/440#issue-2837644226).
* I have done some modifications including replay buffer change, etc.
* You can view availale gym envs in gym [website](https://www.gymlibrary.dev/environments/mujoco/half_cheetah/). But due to spingingup is an old project using an old gym, only old versions of tasks are supported. For instance, you can't use `Hopper-v4`, instead, you should use `Hopper-v2`.
* I reproduced [benchmarks for spiningup implementations](https://spinningup.openai.com/en/latest/spinningup/bench.html?utm_source=chatgpt.com#halfcheetah-pytorch-versions).



**Status:** Maintenance (expect bug fixes and minor updates)

Welcome to Spinning Up in Deep RL! 
==================================

This is an educational resource produced by OpenAI that makes it easier to learn about deep reinforcement learning (deep RL).

For the unfamiliar: [reinforcement learning](https://en.wikipedia.org/wiki/Reinforcement_learning) (RL) is a machine learning approach for teaching agents how to solve tasks by trial and error. Deep RL refers to the combination of RL with [deep learning](http://ufldl.stanford.edu/tutorial/).

This module contains a variety of helpful resources, including:

- a short [introduction](https://spinningup.openai.com/en/latest/spinningup/rl_intro.html) to RL terminology, kinds of algorithms, and basic theory,
- an [essay](https://spinningup.openai.com/en/latest/spinningup/spinningup.html) about how to grow into an RL research role,
- a [curated list](https://spinningup.openai.com/en/latest/spinningup/keypapers.html) of important papers organized by topic,
- a well-documented [code repo](https://github.com/openai/spinningup) of short, standalone implementations of key algorithms,
- and a few [exercises](https://spinningup.openai.com/en/latest/spinningup/exercises.html) to serve as warm-ups.

Get started at [spinningup.openai.com](https://spinningup.openai.com)!


Citing Spinning Up
------------------

If you reference or use Spinning Up in your research, please cite:

```
@article{SpinningUp2018,
    author = {Achiam, Joshua},
    title = {{Spinning Up in Deep Reinforcement Learning}},
    year = {2018}
}
```