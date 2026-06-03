---
layout: page
title: "Risk-Aware Reinforcement Learning with Bandit-Based Adaptation for Quadrupedal Locomotion"
permalink: /papers/risk-aware-locomotion/
nav: false
---

**[Yuanhong Zeng](https://danny-zyh.github.io/)**, **[Anushri Dixit]({{ '/people/anushri_dixit/' | relative_url }})**

*University of California, Los Angeles*

<p>
  <a href="https://arxiv.org/pdf/2510.14338" class="btn btn-sm z-depth-0" role="button">paper</a>
  <a href="https://github.com/practice-lab-ucla/safe_locomotion" class="btn btn-sm z-depth-0" role="button">github</a>
  <a href="{{ '/publications/' | relative_url }}" class="btn btn-sm z-depth-0" role="button">publications</a>
</p>

<style>
  .ral-section-heading {
    margin-top: 2.5rem;
    margin-bottom: 1rem;
    padding: 0.6rem 0.85rem;
    border-left: 5px solid var(--global-theme-color);
    background: rgba(0, 123, 255, 0.08);
  }

  .ral-subsection-heading {
    margin-top: 2rem;
    margin-bottom: 1rem;
    padding-bottom: 0.35rem;
    border-bottom: 2px solid rgba(0, 0, 0, 0.1);
  }

  .ral-figure {
    text-align: center;
    margin: 1.5rem 0;
  }

  .ral-figure img {
    margin-left: auto;
    margin-right: auto;
    display: block;
  }

  .ral-video-wrapper {
    position: relative;
    padding-bottom: 56.25%;
    height: 0;
    overflow: hidden;
    margin: 1.5rem 0;
  }

  .ral-video-wrapper iframe {
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    border: 0;
  }
</style>

<h2 class="ral-section-heading">Video</h2>

<div class="ral-video-wrapper">
  <iframe src="https://www.youtube.com/embed/KRZPw90XldY" allow="autoplay; encrypted-media" allowfullscreen></iframe>
</div>

<h2 class="ral-section-heading">Abstract</h2>

In this work, we study risk-aware reinforcement learning for quadrupedal locomotion. Our approach trains a family of risk-conditioned policies using a Conditional Value-at-Risk (CVaR) constrained policy optimization technique that provides improved stability and sample efficiency. At deployment, we adaptively select the best performing policy from the family of policies using a multi-armed bandit framework that uses only observed episodic returns, without any privileged environment information, and adapts to unknown conditions on the fly. Hence, we train quadrupedal locomotion policies at various levels of robustness using CVaR and adaptively select the desired level of robustness online to ensure performance in unknown environments. We evaluate our method in simulation across eight unseen settings (by changing dynamics, contacts, sensing noise, and terrain) and on a Unitree Go2 robot in previously unseen terrains. Our risk-aware policy attains nearly twice the mean and tail performance in unseen environments compared to other baselines and our bandit-based adaptation selects the best-performing risk-aware policy in unknown terrain within two minutes of operation.

<h2 class="ral-section-heading">Methodology</h2>

We train multiple policies using CVaR-constrained policy optimization, where each critic focuses on a different tail of the return distribution, resulting in policies with varying levels of risk awareness. An upper confidence bound (UCB) bandit is then used to adaptively select the appropriate risk level online. The bandit selects the best performing policy over time as the robot interacts with the environment repeatedly.

<figure class="ral-figure">
  <img src="{{ '/paper_web/risk-aware-locomotion/images/anchor.png' | relative_url }}" alt="System diagram" class="img-fluid rounded z-depth-1" style="width: 480px; max-width: 100%;" loading="lazy">
  <figcaption class="caption">System diagram: CVaR policy training and bandit-based online adaptation.</figcaption>
</figure>

<h2 class="ral-section-heading">Results</h2>

<h3 class="ral-subsection-heading">Risk Awareness</h3>

We commanded the robot to walk at full speed ($v=1\,\text{m/s}$) on both ramp and grass terrains. The $\alpha=0.05$ policy did not advance despite forward commands, either when climbing the ramp or when a foot became trapped in a soccer cone hidden beneath the grass. In contrast, the $\alpha=0.25$ policy adapted a stable gait with larger steps, enabling the robot to step over hidden cones. The PPO baseline moved faster but was more prone to losing balance, particularly when descending the ramp or when one of its feet hits the soccer cone.

<figure class="ral-figure">
  <img src="{{ '/paper_web/risk-aware-locomotion/images/ramp_exp.png' | relative_url }}" alt="Experiment snapshot comparing risk-aware policies on ramp and grass terrain" class="img-fluid rounded z-depth-1" style="width: 640px; max-width: 100%;" loading="lazy">
  <figcaption class="caption">Experiment snapshot: comparing risk-aware policies on ramp and grass terrains.</figcaption>
</figure>

<h3 class="ral-subsection-heading">Bandit Selection</h3>

Cumulative probability of selecting each policy in one ramp experiment. Selection of the policy with $\alpha = 0.25$ quickly dominates after 2000 timesteps, which is roughly 1 minute wall clock time.

<figure class="ral-figure">
  <img src="{{ '/paper_web/risk-aware-locomotion/images/bandit_perf.png' | relative_url }}" alt="Bandit policy selection performance over time" class="img-fluid rounded z-depth-1" loading="lazy">
  <figcaption class="caption">Cumulative policy selection probability: the α=0.25 policy dominates within ~1 minute of operation.</figcaption>
</figure>

<h2 class="ral-section-heading">BibTeX</h2>

```bibtex
@article{zeng2025risk,
  title={Risk-Aware Reinforcement Learning with Bandit-Based Adaptation for Quadrupedal Locomotion},
  author={Zeng, Yuanhong and Dixit, Anushri},
  journal={2026 IEEE International Conference in Robotics and Automation (ICRA)},
  year={2026},
}
```