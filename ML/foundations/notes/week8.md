# 8.1: Pillars of Machine Learning 


## 1. The Three Pillars of Machine Learning

The theory of machine learning rests upon three foundational mathematical pillars. To understand any machine learning problem in detail, an understanding of each of these three pillars is required.

### The Pillars:

1. **Linear Algebra**
2. **Probability**
3. **Optimization**

---

## 2. Motivating Example: Height and Weight Prediction

Consider a dataset capturing the height (in centimeters) and weight (in kilograms) of people in a room. When plotted on a two-dimensional plane, each point represents one particular person.

<img width="333" height="206" alt="image" src="https://github.com/user-attachments/assets/90591739-f954-47b1-9587-8319293fa142" />

### Goal:

If a new person enters the room and their height is known, how can we predict their weight?

### Observations:

- There appears to be a **linear relation** between height and weight.
- As height increases, weight also seems to increase overall on average.
- If a line is found that "best represents" this relation, the weight for a given height can be predicted by finding the corresponding value on that line.

---

## 3. Pillar 1: Linear Algebra (Structure)

Linear algebra is used to understand the **structure or relationship between data**.

- **Assumption:** In the example, we assume height and weight are linearly related.
- **Purpose:** It handles the structural relationship between parameters of interest.
- **Complexity:** While relationships can be non-linear, the linear relationship is the simplest structural model. Understanding linear relationships well often allows for the understanding of non-linear models as well.

---

## 4. Pillar 2: Probability (Uncertainty)

Probability is the language used to **model noise or uncertainty in data**.

- **The Reality of Data:** In the height-weight dataset, not every point falls exactly on a single line.
- **Definition of Noise:** Everything that does not pertain to the linear relationship is considered noise.
- **Sources of Noise:**

  - Measurement errors.
  - Incorrect measurements.
  - Missing measurements.
- **Purpose:** It is essential to not only understand structure but also to handle the uncertainty inherent in real-world data.

---

## 5. Pillar 3: Optimization (Decisions)

Optimization is the mathematical tool that allows us to **convert data to decisions**.

### The Problem of Choice:

Even with a structural model (Linear Algebra) and a noise model (Probability), there isn't a single way to describe the relationship.

- There are **infinite possibilities** for lines that can relate height and weight.
- We need a way to determine which line **best** represents the relationship in the observed data.


### The Role of Optimization:

- **The Notion of "Best":** Optimization is used the moment there is a goal to find the "best" way to do certain things.
- **Action:** It involves picking one specific model (e.g., one specific line) out of infinite possibilities based on a criteria of best fit.
- **Summary:** It is the bridge that turns modeled data into a final decision or prediction.

---

## 6. Conclusion

While linear algebra and probability provide the framework for understanding data structure and noise, optimization provides the mechanism to navigate those possibilities and arrive at a solution. The goal of this course section is to understand the basic ideas of optimization to help navigate the world of machine learning and data science.







---



