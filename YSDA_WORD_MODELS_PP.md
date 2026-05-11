# Uncertainty in Video World Models for Robotic Manipulation

## Research Question

The question of this project is:

> Can we detect when a video world model is uncertain or overconfidently wrong about the outcome of a robot action, especially in grasping and contact-rich manipulation tasks?

More specifically, we want to study:

1. How confident is the world model when it predicts successful grasping?
2. Are failed real-world grasps associated with higher uncertainty in the model?
3. Are there cases where the model is confidently wrong?
4. Can uncertainty signals be used to avoid risky actions before execution?
5. Does uncertainty-aware planning improve success rates compared to standard best-of-N planning?

## Research Hypothesis

The main hypothesis of this project is:

> Failed grasping and contact-rich manipulation outcomes can often be predicted from uncertainty signals in the video world model, value model, or latent representation before the robot executes the action.

Or:

> Uncertainty-aware planning can reduce robot failures by avoiding action candidates whose predicted successful futures are unreliable.

## Methodology

### Data

For each robot state-action pair, we collect:

- current observation `s_t`;
- candidate action chunk `a_t`;
- multiple predicted future states from the world model;
- multiple predicted values from the value model;
- real future observation after executing the action;
- final success/failure label.

A single data point can be represented as:

```text
(s_t, a_t, predicted_futures, predicted_values, real_future, success_label)
```

For each action, generate multiple future predictions:

```text
s'_1, s'_2, ..., s'_K
```

and multiple value predictions:

```text
V_1, V_2, ..., V_K
```

Then compare these predictions with the real outcome.

### Failure Categories

Each evaluated action can be assigned to one of several categories.

| Category | Predicted Value | Uncertainty | Real Outcome | Interpretation |
|---|---:|---:|---:|---|
| Safe Success | High | Low | Success | Model is correct and confident |
| Detected Risk | Medium/Low | High | Failure or mixed | Model is uncertain and risk is visible |
| Detected Failure | Low | Low | Failure | Model correctly predicts failure |
| Overconfident Failure | High | Low | Failure | Model is confidently wrong |
| Ambiguous Case | Medium | Medium/High | Success or failure | Outcome is hard to predict |

The most important category is **Overconfident Failure**, because these are cases where ordinary uncertainty estimation may fail.

### Case 1: Value Uncertainty

The simplest uncertainty score is the variance of predicted values:

```text
U_value = Var(V_1, V_2, ..., V_K)
```

or:

```text
mean_value = mean(V_1, V_2, ..., V_K)
std_value  = std(V_1, V_2, ..., V_K)
min_value  = min(V_1, V_2, ..., V_K)
q10_value  = 10th_percentile(V_1, V_2, ..., V_K)
```

### Case 2: Future-State Uncertainty

Generate multiple future images or videos for the same state-action pair and measure their disagreement.

Possible metrics:

- pixel-level variance;
- LPIPS distance;
- DINO feature variance;
- CLIP feature variance;
- segmentation-mask disagreement;
- object position variance;
- gripper-object distance variance;
- predicted contact-state variance.

A general future disagreement score can be written as:

```text
U_future = average_distance(predicted_future_i, predicted_future_j)
```

However, raw image disagreement may not be sufficient. For grasping, the project should focus on **task-relevant disagreement**.

### Case 3: Detect OOD using latent representations

Possible representations:

- image encoder embeddings;
- video-model latent embeddings;
- action chunk embeddings;
- proprioception embeddings;
- combined state-action embeddings.

An OOD score can be estimated as:

```text
U_ood = distance_to_nearest_training_examples(z(s, a))
```

## Uncertainty-Aware Planning

The baseline Cosmos-style planner chooses the action with the highest expected predicted value:

```text
a* = argmax_a mean(V(a))
```

This project can test safer alternatives.

### Option 1: Penalize Value Variance

```text
score(a) = mean(V(a)) - beta * std(V(a))
```

The planner chooses:

```text
a* = argmax_a score(a)
```

This discourages actions with high value uncertainty.

### Option 2: Lower-Confidence-Bound Planning

```text
score(a) = mean(V(a)) - beta * sqrt(Var(V(a)))
```

This chooses actions that are both high-value and reliable.

### Option 3: Quantile-Based Planning

```text
score(a) = quantile_10_percent(V_1, V_2, ..., V_K)
```

This means the action is selected based on a pessimistic estimate of success.

### Option 4: Contact-Aware Risk Penalty

```text
score(a) = mean(V(a)) - beta * U_value(a) - gamma * U_grasp(a)
```

This is especially suitable for grasping tasks.

### Option 5: OOD-Aware Planning

```text
score(a) = mean(V(a)) - beta * U_value(a) - gamma * U_ood(a)
```

This penalizes actions far from known data.
