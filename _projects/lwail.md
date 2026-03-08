---
layout: page
title:  "Latent Wasserstein Adversarial Imitation Learning" 
date:   2026-03-05 00:00:00 -0800
category: work
related_publications: false
importance: 1
author: Siqi Yang, https://jackyyang258.github.io; Kai Yan, https://kaiyan289.github.io; Alexander G. Schwing, https://alexander-schwing.de; Yu-Xiong Wang, https://yxw.web.illinois.edu
author_affiliation: University of Illinois Urbana-Champaign (UIUC)
---

<script
  src="https://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-AMS-MML_HTMLorMML"
  type="text/javascript">
</script>

<h4 align="center"> ICLR 2026</h4>  
<hr>
<h4 align="center"> <a href="https://arxiv.org/abs/2603.05440">PDF</a> | <a href="https://github.com/JackyYang258/LWAIL">Code</a> </h4>

<h1 align="center">Abstract</h1>

<div class="quote"><p style='font-size:16pt'><i>Traditional distance metrics fail to capture environment dynamics in Wasserstein AIL;<br> we solve this by computing the Wasserstein distance in a dynamics-aware latent space.</i></p></div>
<details>
<summary><b>Full Abstract</b></summary>
Imitation Learning (IL) enables agents to mimic expert behavior by learning from demonstrations. However, traditional IL methods require large amounts of medium-to-high-quality demonstrations as well as actions of expert demonstrations, both of which are often unavailable. To reduce this need, we propose Latent Wasserstein Adversarial Imitation Learning (LWAIL), a novel adversarial imitation learning framework that focuses on state-only distribution matching. It benefits from the Wasserstein distance computed in a dynamics-aware latent space. This dynamics-aware latent space differs from prior work and is obtained via a pre-training stage, where we train the Intention Conditioned Value Function (ICVF) to capture a dynamics-aware structure of the state space using a small set of randomly generated state-only data. We show that this enhances the policy's understanding of state transitions, enabling the learning process to use only one or a few state-only expert episodes to achieve expert-level performance. Through experiments on multiple MuJoCo environments, we demonstrate that our method outperforms prior Wasserstein-based IL methods and prior adversarial IL methods, achieving better results across various tasks.
</details>

<h1 align="center">Why the Distance Metric Fails</h1> 

Many prior Wasserstein IL[1] works that employ the Kantorovich-Rubinstein (KR) dual overlook an important issue: the distance metric between individual states is rather simplistic. For this, the Euclidean distance is common. However, it fails to capture the environment's dynamics. For example, a state might be physically close to an expert state in Euclidean space, but unreachable due to an obstacle, making it a poor metric for the learning process.


<p align="center">
<img
  src="/assets/lwail_teaser.png"
  style="display: block; margin: 0 auto; width: min(100%, 340px); border-radius: 12px; box-shadow: 0 10px 24px rgba(0, 0, 0, 0.16);"
>
<br>
<i>Illustration of a case where the Euclidean distance between states is not a good metric (State B is closer to Expert State C in Euclidean distance, but actually State A is closer to Expert State C in real dynamic world).</i>
</p>

<!-- <h1 align="center">What is Our Solution?</h1>

The following table shows a comparison between our proposed LWAIL and existing methods, where <span style="color:lightgray">gray</span> shows suboptimal design:

---| Geometric property of distributions | Distance metric | Matching Objects | Training Efficiency 
---|---|---|---|---
*f*-divergence-based methods | <span style="color:lightgray">No</span> | <span style="color:lightgray">KL or $\chi^2$</span> | Distributions | Bi-level/Single-level
Prior Wasserstein AIL (KR Dual) | Yes | <span style="color:lightgray">Euclidean</span> | Distributions | <span style="color:lightgray">Metric constrained</span> 
Our method (LWAIL) | Yes | <b>Dynamics-aware Latent Space</b> | Distributions | <b>Bi-level optimization</b> -->


We propose a two-stage process:
* **Pre-training stage:** We leverage a small (1% of online rollouts) number of unstructured, low-quality (e.g., random) state-only data to train an Intention-Conditioned Value Function[2]. The resulting embedding captures a rich, dynamics-aware notion of reachability between states.
* **Imitation stage:** We freeze this ICVF embedding and use the Euclidean distance in this new latent space as the cost function within a standard Wasserstein AIL framework.

In the adversarial imitation learning stage, we optimize the following objective:
<div style="background: #f7fafc; border-left: 4px solid #3a6ea5; border-radius: 8px; padding: 0.85rem 1rem; margin: 1rem 0 1.2rem 0;">

$$
\min_\pi\max_{\|f\|_L\leq 1}
\left(
\mathbb{E}_{(s, s')\sim d^\pi_{ss}}[f(\phi(s),\phi(s'))]
- \mathbb{E}_{(s,s')\sim d^E_{ss}}[f(\phi(s),\phi(s'))]
\right).
$$

where $\pi$ is the policy to be learned, $f$ is the critic constrained by \( \lVert f \rVert_L \le 1 \) (1-Lipschitz), $d^\pi_{ss}$ is the state-transition pair distribution $(s,s')$ induced by policy $\pi$, $d^E_{ss}$ is the expert state-transition pair distribution $(s,s')$, and $\phi(\cdot)$ is the frozen ICVF embedding that maps raw states to the dynamics-aware latent space. Intuitively, the critic maximizes the Wasserstein discrepancy between policy and expert transition-pair distributions in latent space, while the policy minimizes it.
</div>

<h1 align="center">Performance</h1>

We validate our approach on Umaze and challenging locomotion tasks in the MuJoCo environment from the D4RL benchmark, achieving strong results using only a single trajectory of state-based expert data. The results show that the latent space grasps the transition dynamics much better than the vanilla Euclidean distance.

<p align="center">
<span style="display: inline-block; width: 48%;">
  <img
    src="/assets/lwail_tsne_halfcheetah.png"
    style="display: block; margin: 0 auto; width: 100%; border-radius: 12px; box-shadow: 0 10px 24px rgba(0, 0, 0, 0.16);"
  >
</span>
<span style="display: inline-block; width: 48%;">
  <img
    src="/assets/lwail_tsne_walker.png"
    style="display: block; margin: 0 auto; width: 100%; border-radius: 12px; box-shadow: 0 10px 24px rgba(0, 0, 0, 0.16);"
  >
</span>
<br>
<span style="display: inline-block; width: 48%;">tsne_halfcheetah</span>
<span style="display: inline-block; width: 48%;">tsne_walker</span>
<br>
<i>t-SNE visualizations in the original state space and the embedding latent space on HalfCheetah and Walker2d. The ICVF-trained embedding provides a more dynamics-aware metric.</i>
</p>



<h4 align="center">MuJoCo Environments (1 Expert Trajectory)</h4>
<img
  src="/assets/lwail_performance.png"
  style="display: block; margin: 0 auto; width: min(100%, 960px); border-radius: 12px; box-shadow: 0 10px 24px rgba(0, 0, 0, 0.16);"
>
<p align="center"><i>Normalized Rewards (Higher is Better)</i></p>


<h4 style="text-align: left; margin-top: 1rem;">References</h4>

[1] Martin Arjovsky, Soumith Chintala, and L´ eon Bottou. Wasserstein generative adversarial networks. In ICML, 2017.

[2] Dibya Ghosh, Chethan Anand Bhateja, and Sergey Levine. Reinforcement learning from passive data via latent intentions. In ICML, 2023.
