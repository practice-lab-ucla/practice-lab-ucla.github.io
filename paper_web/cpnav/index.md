---
layout: page
title: "CPNAV"
permalink: /papers/cpnav/
nav: false
---

# CPNAV: Calibrated Vision-Language Navigation for Object-Goal Search in Unseen Environments

**[Qizhao Chen]({{ '/people/qizhao_chen/' | relative_url }})**, **[Yuanhong Zeng](https://danny-zyh.github.io/)**, **[Shoh Nishino]({{ '/people/Shoh_Nishino/' | relative_url }})**, and **[Anushri Dixit]({{ '/people/anushri_dixit/' | relative_url }})**

<p>
  <a href="{{ '/paper_web/cpnav/IROS_CP_Nav.pdf' | relative_url }}" class="btn btn-sm z-depth-0" role="button">PDF</a>
  <a href="https://github.com/practice-lab-ucla/CPnav.git" class="btn btn-sm z-depth-0" role="button">Code</a>
  <a href="{{ '/publications/' | relative_url }}" class="btn btn-sm z-depth-0" role="button">Publications</a>
</p>

<img src="{{ '/paper_web/cpnav/CPnav.png' | relative_url }}" alt="CPNAV preview" class="img-fluid rounded z-depth-1">

<style>
  .cpnav-section-heading {
    margin-top: 2.5rem;
    margin-bottom: 1rem;
    padding: 0.6rem 0.85rem;
    border-left: 5px solid var(--global-theme-color);
    background: rgba(0, 123, 255, 0.08);
  }

  .cpnav-subsection-heading {
    margin-top: 2rem;
    margin-bottom: 1rem;
    padding-bottom: 0.35rem;
    border-bottom: 2px solid rgba(0, 0, 0, 0.1);
  }

  .cpnav-figure {
    text-align: center;
  }

  .cpnav-figure img,
  .cpnav-figure video {
    margin-left: auto;
    margin-right: auto;
    display: block;
  }
</style>

<h2 class="cpnav-section-heading">Abstract</h2>

Directing a robot to locate a target object in
unseen environments remains a central challenge in embodied
AI. Vision–Language Model (VLM)–based methods provide
rich semantic reasoning but often produce uncalibrated and
overconfident action decisions. We introduce CPNAV, a navi-
gation framework that combines collision-aware planning with
statistically calibrated VLM action scoring for map-free object-
goal navigation. From RGB-D input, CPNAV constructs a
navigability map, generates collision-free motion primitives, and
queries two VLMs for goal detection and action scoring. The
resulting scores are calibrated offline via conformal prediction
(CP) using oracle actions obtained from a modified tree search,
providing finite-sample guarantees at a user-specified risk level.
The CP layer is model-agnostic and can wrap any VLM
policy. Online, the calibrated threshold prunes the search tree
by filtering unreliable actions with backtracking. Experiments
on the HM3D object-goal navigation benchmark show that
CPNAV improves over the 4 Lets look at the main problem
of the baseline uncalibrated method. The agent is placed at a
random location in an apartment, the goal is to find the plant
a baseline in both reliability and efficiency. In simulation, the
proposed method achieves a relative improvement of 6.17% in
the success rate and 11.34% in the success weighted by path
length (SPL) compared to the non-calibrated baselines. In real-
world deployment on a modified Hiwonder MentorPi robot,
CPNAV achieves relative improvements of 24.99% in Success
Rate and 15.20% in SPL compared to baselines.

<h2 class="cpnav-section-heading">Method</h2>

Our idea in CPNAV is to make vision-language navigation more reliable by combining three components: VLM action scoring, conformal prediction for calibrated safety sets, and backtracking tree search. From RGB-D observations, the robot proposes collision-free actions, scores them with VLMs, filters unreliable choices using a conformal threshold, and then explores the remaining actions in a search tree.

<h3 class="cpnav-subsection-heading">(a) VLM-Based Action Scoring</h3>

<figure class="cpnav-figure">
  <img src="{{ '/paper_web/cpnav/figures/method_a_vlm_action_scoring.png' | relative_url }}" alt="CPNAV VLM-based action scoring pipeline" class="img-fluid rounded z-depth-1" loading="lazy">
  <figcaption class="caption">VLM-based action scoring: RGB-D input, voxel mapping, collision filtering, and projected action candidates.</figcaption>
</figure>

From the RGB-D image, CPNAV first builds a local navigability representation and generates collision-free executable actions. These proposed actions are projected back onto the camera image using the camera intrinsics, so the VLM can reason about concrete choices in the scene. At the same time, the robot records a voxel map that marks explored space and obstacles, helping the action proposer prioritize unexplored regions while enforcing collision safety.

<h3 class="cpnav-subsection-heading">(b) Conformal Prediction</h3>

<figure class="cpnav-figure">
  <img src="{{ '/paper_web/cpnav/figures/method_b_conformal_prediction.png' | relative_url }}" alt="CPNAV conformal prediction layer" class="img-fluid rounded z-depth-1" loading="lazy">
  <figcaption class="caption">Conformal prediction converts VLM scores into a calibrated set of high-confidence actions.</figcaption>
</figure>

The conformal prediction layer calibrates the VLM action scores using an offline calibration set and a user-specified risk level. At runtime, CPNAV filters out actions whose confidence falls below the calibrated threshold, keeping only statistically reliable actions. This calibrated safety set is designed so that, with high probability, the oracle/correct action is not removed during navigation.

<h3 class="cpnav-subsection-heading">(c) Backtracking Tree Search</h3>

<figure class="cpnav-figure">
  <img src="{{ '/paper_web/cpnav/figures/method_c_tree_search.png' | relative_url }}" alt="CPNAV backtracking tree search" class="img-fluid rounded z-depth-1" loading="lazy">
  <figcaption class="caption">Backtracking tree search expands calibrated actions and revisits parent nodes when needed.</figcaption>
</figure>

After conformal filtering, the remaining admissible actions are explored in a BFS-style search tree. Each node represents a visited robot state, and each edge represents an executable action from that state. If the selected action is a turn-around or backtracking command, the robot returns to a parent node and continues expanding untried admissible actions, reducing wasted exploration while preserving a path toward the goal.

<h3 class="cpnav-subsection-heading">(d) Robot Environment Interaction</h3>

<figure class="cpnav-figure">
  <img src="{{ '/paper_web/cpnav/figures/method_d_robot_interaction.png' | relative_url }}" alt="CPNAV robot environment interaction hardware platform" class="img-fluid rounded z-depth-1" loading="lazy">
  <figcaption class="caption">Robot environment interaction: simulation evaluation and real-world deployment on the MentorPi platform.</figcaption>
</figure>

CPNAV is evaluated in simulation using AI Habitat with the HM3D dataset, and validated in the real world on a modified Hiwonder MentorPi M1 robot. The hardware platform uses a ZED 2i stereo camera for RGB-D perception, a Mecanum drivetrain for omnidirectional motion, and onboard compute/control hardware for deployment.

<h2 class="cpnav-section-heading">Experiments</h2>

<h3 class="cpnav-subsection-heading">Simulation Experiments</h3>

The simulation experiments are formulated as object-goal navigation episodes in AI Habitat with the HM3D dataset. The agent starts in an unseen indoor scene and must find the target object within a fixed step budget. The paper evaluates success rate (SR), success weighted by path length (SPL), shorter-horizon success/SPL under 50 and 100 steps, and mean travel distance.

CPNAV is compared against four alternatives: Simple Set, Ensemble Set, Prompt Set, and No Calibration. The first three are alternative action-set construction strategies, while No Calibration keeps the same navigation pipeline without the conformal prediction threshold. CPNAV uses the calibrated threshold reported in the paper and achieves the best SR, SPL, short-horizon metrics, and mean distance among the compared policies.

<figure class="cpnav-figure">
  <img src="{{ '/paper_web/cpnav/figures/simulation_results.png' | relative_url }}" alt="Simulation experiment results comparing CPNAV with Simple Set, Ensemble Set, Prompt Set, and No Calibration" class="img-fluid rounded z-depth-1" loading="lazy">
  <figcaption class="caption">Simulation results on AI Habitat/HM3D. CPNAV is compared against Simple Set, Ensemble Set, Prompt Set, and No Calibration.</figcaption>
</figure>

<h3 class="cpnav-subsection-heading">Hardware Experiments</h3>

Following the hardware setup described in the paper, CPNAV was evaluated on a modified Hiwonder MentorPi M1 mobile robot with a Mecanum drivetrain for omnidirectional motion. State estimation used SLAM Toolbox by fusing a 2D LiDAR scanner, wheel odometry, and an IMU, while a ZED 2i stereo camera provided rectified RGB-D observations for perception. The paper reports 24 randomized object-goal episodes across two residential apartment layouts, comparing the uncalibrated baseline against CPNAV with a calibrated threshold.

<figure class="cpnav-figure">
  <video class="img-fluid rounded z-depth-1" controls preload="none" poster="{{ '/paper_web/cpnav/CPnav.png' | relative_url }}" playsinline width="100%">
    <source src="{{ '/paper_web/cpnav/videos/hardware_demo.mp4' | relative_url }}" type="video/mp4">
  </video>
  <figcaption class="caption">
    Hardware demonstration showing the rollout views used to compare navigation behavior.
    <a href="{{ '/paper_web/cpnav/videos/hardware_demo.mp4' | relative_url }}">Open video directly</a>.
  </figcaption>
</figure>

The hardware rollouts below compare the baseline and CPNAV in the same object-goal navigation setting. The baseline commits to less reliable branches, exceeds the maximum step limit, and fails to reach the goal. CPNAV uses calibrated VLM scores to choose more reliable actions and reaches the goal in 14 steps.

<div class="row">
  <div class="col-md-6">
    <figure>
      <img src="{{ '/paper_web/cpnav/figures/baseline_hardware.gif' | relative_url }}" alt="Baseline hardware rollout" class="img-fluid rounded z-depth-1" loading="lazy">
      <figcaption class="caption">Baseline rollout with selected action and voxel map.</figcaption>
    </figure>
  </div>
  <div class="col-md-6">
    <figure>
      <img src="{{ '/paper_web/cpnav/figures/cpnav_hardware.gif' | relative_url }}" alt="CPNAV hardware rollout" class="img-fluid rounded z-depth-1" loading="lazy">
      <figcaption class="caption">CPNAV rollout with calibrated action selection.</figcaption>
    </figure>
  </div>
</div>

In these hardware trials, CPNAV improves the robot's reliability and path efficiency by pruning low-confidence branches during exploration. The reported success rate increases from 66.67% to 83.33%, SPL improves from 0.4204 to 0.4843, and mean distance traveled decreases from 9.0330 m to 6.2949 m.

<figure class="cpnav-figure">
  <img src="{{ '/paper_web/cpnav/figures/hardware_results.png' | relative_url }}" alt="Hardware experiment results comparing No Calibration and CPNAV" class="img-fluid rounded z-depth-1" style="width: 50%; max-width: 50%; height: auto;" loading="lazy">
  <figcaption class="caption">Hardware results on the MentorPi M1 robot. CPNAV improves success rate, SPL, and travel distance relative to No Calibration.</figcaption>
</figure>

<h4 class="cpnav-subsection-heading">Search Tree Efficiency</h4>

These offline tree visualizations show the same experiment as the hardware GIFs above, but as search trees. The baseline explores a broader and more diffuse tree, expanding several low-value branches before exceeding the step limit. CPNAV uses conformal prediction to calibrate vision-language action scores, so unreliable actions can be filtered during tree search. The resulting tree is more focused: fewer branches are explored, and the search progresses more directly toward the goal.

<div class="row">
  <div class="col-md-6">
    <figure>
      <img src="{{ '/paper_web/cpnav/figures/baseline_search_tree.png' | relative_url }}" alt="Baseline offline search tree" class="img-fluid rounded z-depth-1" loading="lazy">
      <figcaption class="caption">Baseline search tree with broader exploration.</figcaption>
    </figure>
  </div>
  <div class="col-md-6">
    <figure>
      <img src="{{ '/paper_web/cpnav/figures/cpnav_search_tree.png' | relative_url }}" alt="CPNAV offline search tree" class="img-fluid rounded z-depth-1" loading="lazy">
      <figcaption class="caption">CPNAV search tree with calibrated, more efficient exploration.</figcaption>
    </figure>
  </div>
</div>

<h3 class="cpnav-subsection-heading">Sensitivity Study</h3>

The paper also studies how CPNAV changes under sensing and modeling variations. This includes camera height, camera field of view, the VLM model used for action scoring, and whether the voxel map is included. The study shows that sensing geometry and model capacity matter: lower field of view, less informative camera placement, a smaller VLM, or removing the voxel map all reduce reliability or path efficiency. CPNAV remains the strongest configuration, indicating that conformal calibration works best when paired with informative RGB-D perception and the voxel map used by the action proposer.

<figure class="cpnav-figure">
  <img src="{{ '/paper_web/cpnav/figures/sensitivity_study.png' | relative_url }}" alt="CPNAV sensitivity study over sensing configuration, model capacity, and voxel map ablation" class="img-fluid rounded z-depth-1" style="width: 70%; max-width: 70%; height: auto;" loading="lazy">
  <figcaption class="caption">Sensitivity study over sensing configuration, model capacity, and voxel-map ablation.</figcaption>
</figure>
