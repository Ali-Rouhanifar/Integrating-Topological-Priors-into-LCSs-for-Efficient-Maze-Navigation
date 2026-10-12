Graph-XCS: Integrating Topological Priors into Learning Classifier Systems for Efficient Maze Navigation

Ali Rouhanifar

Department of Computer Science
Meybod University, Iran
Email: ali.rouhanifar@iau.ac.ir

Abstract

Navigation in static, discrete environments remains a foundational benchmark in reinforcement learning (RL). Although the eXtended Classifier System (XCS) has demonstrated competence in such tasks, its reliance on flat, Markovian state representations limits its capacity to exploit the topological regularities inherent to structured spaces. This deficiency manifests as slow convergence, inefficient exploration, and rule-base bloat. This paper introduces Graph-XCS, a novel architecture that embeds graph-derived topological priors into the XCS learning loop. Specifically, Graph-XCS augments classifier conditions with graph-theoretic features— shortest-path distance to the goal and local connectivity—and biases action selection through a graph-informed heuristic. We conduct a controlled empirical comparison against Standard
XCS in a 10×10 maze environment. To isolate the contribution of each mechanism, we evaluate three ablations: (i) graph
features without heuristic bias (β = 1), (ii) heuristic bias with raw state (β < 1), and (iii) the full Graph-XCS. Results across
[insert N] independent runs demonstrate that Graph-XCS reduces steps-to-goal by [insert metric, e.g., 73%] during early
learning and stabilizes at a rule-base approximately [insert metric, e.g., 67%] smaller than Standard XCS. Critically, ablations
reveal that the heuristic bias drives rapid early convergence, whereas graph-based state representation is responsible for
long-term rule compactness. We further show that GraphXCS generalizes to a structurally distinct test maze without
retraining, whereas Standard XCS requires re-learning. These findings establish topological priors as a principled inductive
bias for LCS in structured environments.

Index Terms—Learning Classifier Systems, XCS, Graph Neural Networks, Reinforcement Learning, Maze Navigation, Inductive Bias
I. Introduction Learning Classifier Systems (LCS) are evolutionary rulebased learners that combine reinforcement learning with
a genetic algorithm to evolve a population of conditionaction rules [1], [2]. The eXtended Classifier System
(XCS) [2], [3] advanced this paradigm by introducing accuracy-based fitness, thereby promoting classifiers that
are both accurate and maximally general. XCS has since become a standard benchmark for rule-based RL and has been successfully applied to data mining, function approximation, and autonomous agent control [4], [5].
Despite these successes, Standard XCS operates on a flat, Markovian representation of state in which each grid cell is treated as an independent entity. In spatially structured environments—such as a 10 × 10 maze—this design is fundamentally limiting. The agent must learn the value of each state-action pair in isolation, unable to exploit the topological relationships (connectivity, distance to-goal, obstacle adjacency) that define the problem’s
structure. Two consequences follow. First, convergence is slow, particularly under sparse rewards, because the agent must discover the goal through undirected exploration. Second, the rule-base grows to include many locationspecific classifiers, each applicable to a single cell, thereby undermining the generalization that is the hallmark of LCS.

The central premise of this work is that topological structure should be an explicit part of the classifier’s hypothesis space, not merely an implicit property of the environment. We propose Graph-XCS, an architecture that models the environment as a graph G = (V, E), where V denotes navigable cells and E denotes valid transitions.
This graph is used in two distinct ways: (i) to augment classifier conditions with graph-theoretic features,
enabling generalization across topologically equivalent
states, and (ii) to bias action selection through a distanceto-goal heuristic, accelerating early exploration.
