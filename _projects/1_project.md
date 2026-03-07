---
layout: page
title: Latent Wasserstein Adversarial Imitation Learning (LWAIL)
description: State-only imitation learning using a dynamics-aware latent space
img: assets/img/lwail_background.jpg
importance: 1
category: work
related_publications: true
---

[cite_start]Imitation Learning (IL) enables agents to mimic expert behavior by learning from demonstrations[cite: 13]. [cite_start]However, a major challenge in IL is the frequent lack of expert actions, encouraging adoption of Imitation Learning from Observations (LfO), which uses only sequences of expert states[cite: 24]. [cite_start]Because even state-only demonstrations are often expensive to acquire, it is crucial to develop methods that can learn from a minimal amount of expert data[cite: 25]. 

[cite_start]Many Adversarial Imitation Learning (AIL) methods measure distribution difference by f-divergence, but these require "distribution coverage", meaning the distributions must be on the same support set to avoid numerical error[cite: 27, 28]. [cite_start]To address these limitations, the Wasserstein distance has gained popularity[cite: 31]. [cite_start]However, many prior works rely on a simplistic Euclidean distance metric between individual states[cite: 32]. [cite_start]Unfortunately, Euclidean distance fails to capture the environment's dynamics[cite: 62].

[cite_start]To solve this, we introduce **Latent Wasserstein Adversarial Imitation Learning (LWAIL)**, a novel adversarial imitation learning framework that focuses on state-only distribution matching[cite: 15]. 

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/lwail_fig1a.jpg" title="Distance Metric Motivation" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/lwail_fig1b.jpg" title="Pre-training Stage" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/lwail_fig1c.jpg" title="Imitation Stage" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Left: Euclidean distance fails to capture true reachability. Middle: Pre-training an Intention-Conditioned Value Function (ICVF) to learn a dynamics-aware embedding. Right: The imitation stage where the agent learns using the frozen embedding space.
</div>

### How It Works

[cite_start]LWAIL learns a distance metric that encodes the environment's dynamics from a minimum amount of state-only low-quality data[cite: 64]. [cite_start]We propose a two-stage process[cite: 66]:

1. [cite_start]**Pre-training:** We leverage a small number of unstructured, low-quality (e.g., random) state-only data to train an Intention-Conditioned Value Function (ICVF)[cite: 66]. [cite_start]The resulting embedding captures a rich, dynamics-aware notion of reachability between states[cite: 67]. [cite_start]The value function is structured as follows[cite: 107]:
[cite_start]$$V_{\theta}(s,s_{+},z)=\phi_{\theta}(s)^{T}T_{\theta}(z)\psi_{\theta}(s_{+})$$ [cite: 108]
2. [cite_start]**Imitation:** During the imitation stage, we freeze this ICVF embedding and use the Euclidean distance in this new latent space as the cost function within a standard Wasserstein AIL framework[cite: 68]. [cite_start]The problem can be addressed via[cite: 202]:
[cite_start]$$min_{\pi}max_{||f||_{L}\le1}(\mathbb{E}_{(s,s^{\prime})\sim d_{ss}^{\pi}}[f(\phi(s),\phi(s^{\prime}))]-\mathbb{E}_{(s,s^{\prime})\sim d_{ss}^{E}}[f(\phi(s),\phi(s^{\prime}))])$$ [cite: 205]

<div class="row justify-content-sm-center">
    <div class="col-sm-6 mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/lwail_tsne_halfcheetah.jpg" title="HalfCheetah Latent Space" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm-6 mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/lwail_tsne_walker2d.jpg" title="Walker2d Latent Space" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    [cite_start]t-SNE visualization of the same trajectory in the original state space and the embedding latent space[cite: 176]. [cite_start]The latent space grasps the transition dynamics much better than the vanilla Euclidean distance[cite: 71].
</div>


### Results

[cite_start]By modifying the agent and discriminator to operate in this learned space, we fix the core issue of prior methods[cite: 69]. [cite_start]We find that this simple approach allows an agent to effectively match the expert's state distribution, enabling highly efficient imitation from minimal expert data[cite: 70]. 

[cite_start]We achieve strong results using only a single trajectory of state-based expert data[cite: 72, 75]. [cite_start]Through experiments on multiple MuJoCo environments, we demonstrate that our method outperforms prior Wasserstein-based IL methods and prior adversarial IL methods[cite: 19]. 

[cite_start]Furthermore, LWAIL demonstrates remarkable robustness in navigation tasks with perturbed initial states[cite: 311]. [cite_start]The ICVF embeddings in LWAIL successfully capture the environment's dynamics, allowing the agent to handle unseen observations and recover from unfamiliar states[cite: 315].

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/lwail_mujoco_curves.jpg" title="MuJoCo Performance" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Performance on the MuJoCo environments. [cite_start]Overall, our method consistently delivers strong performance across all tasks[cite: 279].
</div>