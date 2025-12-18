# Failure Modes in Vision Models  
## Bias, Fragility, and the Limits of Accuracy

### Overview

Modern computer vision models frequently report impressive accuracy on benchmark datasets.  
However, accuracy alone reveals very little about *where* models fail, *for whom* they fail, and *how* fragile their decision-making truly is.

This repository presents a unified empirical study of two closely related failure modes in vision models:

- **Uneven error distribution (bias across classes)**
- **Adversarial fragility under imperceptible perturbations**

Both analyses are conducted using a standard ResNet-18 architecture trained on the SVHN (Street View House Numbers) dataset.  
No architectural modifications or defensive techniques are introduced. The objective is to examine how failure emerges under otherwise conventional training conditions.

---

## Project Scope

Rather than introducing novel models or defenses, this work focuses on *evaluation* — specifically, identifying where common assumptions about reliability break down.

The analyses address two core questions:

1. **Who bears the cost of model errors when accuracy is high?**
2. **How robust are model decisions to changes humans cannot perceive?**

---

## Part I: Bias as Uneven Error Distribution

### When Accuracy Isn’t the Whole Story

The model achieves strong overall test accuracy (approximately 94%), suggesting reliable performance at first glance.  
However, per-class evaluation reveals systematic disparities:

- Some digits (e.g. 0, 1, 2) are consistently classified with high accuracy
- Others (notably 8 and 9) exhibit significantly higher error rates

These failures are not random. They persist across runs and follow structured confusion patterns.

Bias is defined here not in demographic terms, but as **unequal reliability** — when certain classes disproportionately absorb model errors despite high aggregate performance.

---

### Explanation Quality Under Bias

Grad-CAM visualizations reveal further asymmetry:

- Easier classes exhibit compact, well-aligned saliency
- Harder classes show diffuse and fragmented attention

Even when predictions are correct, explanations degrade for already vulnerable classes, suggesting uneven confidence and transparency.

---

## Part II: Adversarial Fragility and Robustness

### When Small Changes Break Vision

Using the same trained model, robustness was evaluated under a Fast Gradient Sign Method (FGSM) adversarial attack.

Key characteristics of the attack:
- Perturbations are imperceptible to humans
- True labels remain unchanged
- Only the model’s decision boundary is exploited

Despite strong baseline accuracy, performance collapses rapidly as perturbation strength increases, approaching random guessing.

---

### Human Perception vs Model Reasoning

Clean and adversarial images appear identical to human observers:
- Shape and semantic content are preserved
- Visual structure remains intact

Yet the model:
- Changes predictions confidently
- Exhibits systematic failure rather than uncertainty

This highlights a fundamental mismatch between human perception and model decision-making.

---

## Interpretation

These two failure modes are deeply connected.

Bias reveals *where* errors concentrate under standard conditions.  
Adversarial fragility reveals *how easily* decision boundaries can be exploited.

Together, they demonstrate that:
- High accuracy does not imply uniform reliability
- Models can fail confidently and systematically
- Robustness and fairness cannot be inferred from aggregate metrics

---

## Key Takeaways

- Accuracy masks structured failure
- Some classes consistently receive worse performance
- Explanation quality degrades unevenly
- Small, imperceptible perturbations can induce confident failure
- Robust evaluation requires counterfactual and adversarial testing

---

## Reproducibility

All experiments were conducted using:
- Dataset: SVHN
- Architecture: ResNet-18
- Training: Standard supervised learning
- Evaluation: Per-class accuracy, Grad-CAM, FGSM attacks

The notebook included in this repository reproduces all results presented.

---

## Final Note

Vision models do not fail loudly.  
They fail unevenly, confidently, and often invisibly.

Understanding *where* and *why* these failures occur is a prerequisite for deploying models responsibly in real-world systems.
