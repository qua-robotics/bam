<p align="center">
  <img src="docs/_static/BAM_logo.png" alt="BAM logo" width="60%">
</p>

# Change log
・2026-08-19 add Feetech STS3032 params
   
    identification in pyproject.toml
      "PyQt5==5.15.11", "PyQt5-Qt5==5.15.2"

Record in Windows\
  length: 0.05, 0.07, 0.1 m\
  mass: 0.259, 0.3247, 0.457 kg

for example
```bash
uv run python -m bam.feetech.all_record --port COM24 --id 1 --motor sts3032 --mass 0.259 --length 0.05 --vin 6.0 --logdir data_raw
```
make "data_processed" folder and
```bash
uv run python -m bam.process --raw data_raw --logdir data_processed --dt 0.005
uv run python -m bam.plot --actuator sts3032 --logdir data_processed

uv run python -m bam.fit --actuator sts3032 --model m1 --logdir data_processed --output bam/params/feetech_sts3032_6_0V/m1.json
uv run python -m bam.plot --actuator sts3032 --logdir data_processed --sim --params bam/params/feetech_sts3032_6_0V/m1.json
```
to mujoco parameter
```bash
uv run python -m bam.to_mujoco --params bam/params/feetech_sts3032_6_0V/m1.json --kp 32 --vin 6.0
```

# BAM: Better Actuator Models

Accurate models of servo actuators are essential for the simulation of robotic systems. It is particularly important while performing Reinforcement Learning (RL) on real robots, as the precision of the model impacts directly the transferability of the learned policy.

The friction model generally implemented in widely used simulators like MuJoCo or IsaacGym is the Coulomb-Viscous, which is too simplistic to accurately represent complex friction phenomena like Stribeck, load-dependence or quadratic effects (read [this article](https://arxiv.org/pdf/2410.08650v1) for more details).

**BAM** aims at making servo-actuator simulation more faithful by:

- proposing an identification pipeline to fit friction models from recorded trajectories,
- providing a set of extended friction models that capture complex friction phenomena,
- sharing a library of identified models for common servos:
  - Dynamixel MX-64
  - Dynamixel MX-106
  - Dynamixel XL-320
  - Dynamixel XL330-288-T
  - eRob80:50
  - eRob80:100
  - Feetech STS3215
- providing a simple API to use these models in MuJoCo CPU and MuJoCo Warp.

## 📖 Documentation

Please refer to the [documentation](https://bam.readthedocs.io/en/latest/) for more details on how to use BAM and the provided friction models.

## 📚 Citation

If you use BAM or the friction models provided in this repository for your scientific work, please cite the following publication:

```bibtex
@inproceedings{duclusaud2025extended,
  title={Extended Friction Models for the Physics Simulation of Servo Actuators},
  author={Duclusaud, Marc and Passault, Gr{\'e}goire and Padois, Vincent and Ly, Olivier},
  booktitle={2025 IEEE International Conference on Robotics and Automation (ICRA)},
  pages={12091--12097},
  year={2025},
  organization={IEEE}
}
```

If you also wish to cite the software directly, you may use:

```bibtex
@software{BAM,
  title = {{BAM: Better Actuator Models}},
  author = {Duclusaud, Marc and Passault, Grégoire},
  license = {Apache-2.0},
  url = {https://github.com/Rhoban/bam},
  version = {0.0.1},
  year = {2024}
}
```
