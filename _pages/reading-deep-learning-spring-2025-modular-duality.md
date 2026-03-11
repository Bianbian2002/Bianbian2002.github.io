---
layout: page
permalink: /reading/deep-learning-spring-2025/modular-duality/
title: Modular Duality in Deep Learning
description: Session notes for Modular Duality in Deep Learning from the Spring 2025 deep learning reading group.
nav: false
---

<p><a href="{{ '/reading/deep-learning-spring-2025/' | relative_url }}">&larr; Back to deep learning reading group</a></p>

## Session Information

- **Session date:** May 11, 2025
- **Presenter:** Zeyu
- **Paper:** [Modular Duality in Deep Learning](https://arxiv.org/abs/2410.21265)
- **Slides:** [5.12 Modular Duality in Deep Learning.pdf]({{ '/assets/pdf/reading/5.12_Modular_Duality_in_Deep_Learning.pdf' | relative_url }})

## Papers for Discussion

Main paper:

1. [Modular Duality in Deep Learning](https://arxiv.org/abs/2410.21265)

Companion papers:

1. [Scalable Optimization in the Modular Norm](https://arxiv.org/abs/2405.14813)
2. [Muon is Scalable for LLM Training](https://arxiv.org/abs/2502.16982)
3. [Feature Learning in Infinite-Width Neural Networks](https://arxiv.org/abs/2011.14522)

## Slides for Presentation

- [5.12 Modular Duality in Deep Learning.pdf]({{ '/assets/pdf/reading/5.12_Modular_Duality_in_Deep_Learning.pdf' | relative_url }})

## Other Related Papers

1. [Muon Optimizer Accelerates Grokking](https://arxiv.org/abs/2504.16041)
2. [Shampoo: Preconditioned Stochastic Tensor Optimization](https://arxiv.org/abs/1802.09568)

## Minor Issues and Questions

1. [Zeyu] Can we think about dualizing the gradient as a kind of preconditioning method? The intuition seems similiar.
2. [Zeyu] Why $dualize_{\|\cdot\|}$ is well defined? (i.e. different maximizer should lead to the same training dynamics)
   - [Zeyu] The maximizer corresponds to steepest descent direction set. I'm wondering would we favor "easier"/"compact" maximizer for good generalization?
3. [Zhi] why does "the value of Equation (13) may lie in its action on the small singular values" (page 8)? My understanding is that:
   1. the dual $UV^T$ has all singular values equal to 1, eliminating the singular value magnitude difference in the gradient, so *the dualized/preconditioned gradient has a more uniform singular value distribution, leading to a maximal stable rank for $\Delta W$*
   2. which also explains the claim "Linear.dualize ramps up the stable rank of updates, so the weights should move a non-trivial relative amount at large width even in the Frobenius norm, provided the batch size is not too small" because:
      1. **[weights move a roughly constant amount at any width when the change is measured in spectral norm (Yang et al., 2023)]** + **[stable rank maximization]** $\Rightarrow \Delta W$ should have a relatively large Frobenius norm
   3. not sure how to formalize this and I have a feeling that these duality maps can be used to accelerate grokking since it seems to forbid the training from being lazy in Frobenius norm and steers trajectory away from one-singular-value-dominated training
4. [Zhi] what role does module mass play mathematically? It's a bit confusing how the norm or duality map of the module composition or concatenation is derived.
   1. [Zeyu] Basically it controls the feature learning proportion.
   2. The norm is defined such that supermodule preserve well-normedness, and the duality map could be derived by considering maximizing two added terms.
5. [Zhi] how is the norm picked for the input and output space of a certain module? (e.g. why is it $L_1 \to RMS$ in embedding)?
   1. [Tianhao] +1. Is it because for the embedding layer, the input is essentially one-hot vectors? (It's like an embedding table.) But what I don't understand is why using the $\ell_1$ norm, since all norms are basically equivalent for one-hot vectors. In general, it seems vague what they refer to as "semantics" of a layer.
   2. [Zeyu] The paper [Training Deep Learning Models with Norm-Constrained LMOs](https://arxiv.org/abs/2502.07529) argues you could choose different norm for one's convenience.
6. [Tianhao] What does it mean by "feature learning" in this paper?
   1. [Zeyu] I think it is by characterizing linear change of forward passing.
7. [Binghua] Does this paper practically perform better than current methods? It seems that they do not have experimental evidence of this methodology in the main text except for a record made by others (?)
8. [Mengzhe] I am confused about the following definition.
   - [Zeyu] I think a more decent notation would be:
9. [Shuangning] How does the current method compare to the normed optimization method? In what cases is one more desirable than the other?
   1. Current method:
   2. Normed optimization:

## Main Comments and Questions

1. [Zeyu] How to specify mass parameters in practice?
   1. [Zeyu] Found some clues in [Scalable Optimization in the Modular Norm](https://arxiv.org/abs/2405.14813)
2. [Tianhao] Can we connect this to Averaged Gradient Outer Product (AGOP) which characterizes feature learning?
3. [Binghua] Can we develop any regret bound (given mild conditions on loss) on this algorithm proposed in [Scalable Optimization in the Modular Norm](https://arxiv.org/abs/2405.14813)?
   1. [Binghua] It might be very difficult when the structure of each module is complex. Can we develop some upper bound in some naive loss functions and naive module structure (concatenation, composition, etc.)?
4. [Mengzhe] I am wondering whether it is worthwhile for us to define new modules and new ways of operation. What is the scope of their theoretical analysis?
   1. [Binghua] I think Proposition 5 in [Scalable Optimization in the Modular Norm](https://arxiv.org/abs/2405.14813) might be one of the theoretical reasons. I'm not sure but it seems to me that the proposition says that mild landscape properties in small module can be extended to the total module. But it seems there is still a gap between the statement of Proposition 5 and why do they choose the normed optimization.
5. [Shuangning] I wonder whether there are ways to adaptively learn the mass parameters during training, that is, to let the model learn from data which modules should be more important.
6. [Tianhao] This framework somehow can be viewed as a version of dimensional analysis from physics. (量纲分析)

## Ideas and Thoughts for Future Direction

1. [Tianhao] The steepest descent framework seems incompatible with momentum. How to understand the effect of momentum in the dual space?
2. [Tianhao] Can we view the dataset as another module??

## Meeting Notes

1. What's the sensitivity of a bound module? Same as atomic modules: it's still the Lipschitz constant.
2. What do we mean by feature learning? This is discussed in Proposition 3 of the paper *Scalable Optimization in the Modular Norm*. The mass controls the proportion of each module's contribution to the linearized change in the compound module.
3. Connection to preconditioning: The method in this paper seems unrelated to the data, whereas preconditioning typically places more emphasis on data characteristics.
4. When there are multiple maximizers, it seems reasonable to set the irrelevant dimensions to zero. There doesn't appear to be any benefit in choosing other values in those dimensions.
5. What exactly is feature learning in this context? What role does the mass play, and how should it be chosen? Figure 3 in *Scalable Optimization in the Modular Norm* illustrates how the optimization algorithm performs under different mass choices. The paper also argues that the $L_\infty - L_\infty$ operator normalization has the convenient feature that it decouples over matrix rows, making it more local than spectral normalization and, dare-we-say, more biologically plausible. However, this still remains somewhat unclear to us.
6. Bond module: We discussed how the bond module appears to have zero mass. The paper discusses the sensitivity of the ReLU bond module, claiming it is $1/\sqrt{2}$, which seems questionable in general, especially when the signs are not balanced.
7. Physical analogy and dimensional analysis: It would be interesting to think of each module as a resistor and consider combining them in series or parallel. A dimensional analysis (量纲分析) could reveal interesting connections.
