---
layout: about
title: "Home"
permalink: /
# subtitle: <a href='#'>Affiliations</a>. Address. Contacts. Motto. Etc.
# lab_logo: /assets/img/lab_logo.png
# banner_image: /assets/img/1.jpg

gallery:
  - /assets/img/meeting_1.jpg
  - /assets/img/subterrain_robot.png

selected_papers: false # includes a list of papers marked as "selected={true}"
social: false # includes social icons at the bottom of the page

announcements:
  enabled: true # includes a list of news items
  scrollable: true # adds a vertical scroll bar if there are more than 3 news items
  limit: 5 # leave blank to include all the news in the `_news` folder
---

{% include carousel.liquid %}

Welcome to the `PRACTICE (Probabilistic Robotics and Control Theory in Complex Environments)` Lab. Our lab develops verifiable algorithms for safety-critical autonomous systems. These systems must be able to perceive their dynamic and uncertain environments to enable safe and intelligent decision-making in hazardous or sensitive environments such as in search and rescue operations, extraterrestrial exploration, and urban driving. 

Towards this goal, we have developed new theoretical foundations for risk-aware, stochastic motion planning to account for diverse and varying uncertainty descriptions while retaining the tractability of state-of-the-art approaches. This research has been used for search and rescue tasks in the [DARPA Subterranean Challenge](https://www.darpa.mil/research/programs/darpa-subterranean-challenge) as a part of the [Jet Propulsion Lab Team CoSTAR’s solution](https://costar.jpl.nasa.gov/#subT_up). We have developed tools for uncertainty quantification of learning-based systems such as [perception systems](https://perceive-with-confidence.github.io), [large and vision language model-based planners](https://robot-help.github.io) to ensure safe and reliable planning.
