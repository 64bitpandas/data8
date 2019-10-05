# Notes about Statistics
The stats part of data science

## Probability
**Multiplication Rule:** P(A and B) = P(A) * P(B|A)
 - Always less than or equal to P(A)

**Addition Rule:** P(A or B) = P(A) + P(B) - P(A and B)

**Complement Rule:** P(A) = 1 - P(A⁻¹)
 - The **Complement (A⁻¹)** is the event(s) that occurs if A does NOT occur.

## Sampling
 - **Deterministic Sample:** No chance involved, systematic selection used instead
    - Example: Using school directory and selecting every 10th person by alphabetical order, choosing flights that only go to New York, etc.
 - **Random Sample:** 
     - Not all groups necessarily need to have an equal chance of being selected
     - population and chance of selection for each group in the sample must be well defined
 - **Convenience Sample:** Sample the easiest/simplest group available
    - Issue: will not be representative of population of interest

## Distributions
 - **Probability Distribution (aka Samping Distribution):** A random quantity with various possible values
    - The "distribution" itself is a list of the probability of *all* possible values of the quantity
    - Simulation is the easiest way to represent a probability distribution
    - Can be hard to calculate because all possible samples must be calculated
 - **Empirical Distribution:** Based on observations, typically from repititions of an experiment
 - **Law of Averages/Large Numbers:** As the number of times an experiment is done increases, the empirical distribution will look closer to the theoretical distribution

## Inference
 - **Statistical inference:** Making conclusions based on data in random samples. 
   - Used to guess the value of an unknown number.
 - **Parameter:** A number (Median, Mean, etc.) *associated* with a *population*.
   - Usually a statistic is used to estimate a parameter. 
   - Each estimate will likely be different every time you sample.
 - **Statistic:** A number *calculated* from a *sample*
   - Larger sample sizes (most of the time) give you more accurate values for the statistics. The probability distribution of the statistic will be centered around the parameter. 
   - Statistics can have their own probability distributions. 
   - If sampling *without* replacement, then the value of the statistic will approach the value of the parameter as the size of the sample approaches the whole population
- **Models:** A set of assumptions about the data. 
   - Most involve assumptions about randomness
   - Key Question: Does the model fit the data?
- **Assessment:** Simulation of a model under the assumption of the model in order to try to see if the model fits the actual data.

