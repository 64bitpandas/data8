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
 - **Increasing Sample Size:** As sample size increases, the values of the statistics will approach the true value of the parameter.
 - **Increasing Number of Samples:** The shape of the distribution should stay the same, but lower amounts of samples will lead to slightly more noise.

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
 - **Total Variation Distance:** `sum(abs(dist1-dist2))/2` where `dist1` and `dist2` are the values observed from Distribution 1 and Distribution 2. Typically, one of these distributions is a model and the other is observed data.
   - Lower TVD means the two distributions are more similar. TVD is always non-negative.
   - TVD is only used to compare two *categorical* distributions.
   - Compare the *calculated TVD* (using the formula above) with the *simulated TVD* (creating a distribution by randomly selecting a distribution according to the model using `sample_proportions()`)


## Models
   - **Definition:** A set of assumptions about the data. 
      - Most involve assumptions about randomness
      - Key Question: Does the model fit the data?
      - Compare model prediction to the actual data. If they do not match, the model is likely incorrect.
      - Example of a model: people selected at random. (Were they *really* randomly selected, or was there bias?)
   - **Assessment:** Simulation of a model under the assumption of the model in order to try to see if the model fits the actual data.
      -Choose a statistic to measure the accuracy of a model.
      -Simulate the statistic under the model's assumptions
      -Compare the experimental data to the model's prediction.
      -Example: Use values such as experimental TVD and observed TVD under the model's assumptions.
   - **Alternative:** The opposite claim that you make when you propose a model.
      -Example: The people were not a random sample.

## Hypotheses and Testing
   - **Null Hypothesis:** A well defined chance model about how the data was generated. We can simulate data *under the null hypothesis*.
      - Simulate under the null hypothesis will give and emperical distribution of the statistic under the null hypothesis. If the observed data is not consistent with the distribution, then the alternative is favored.
      - Use a visualization to verify consistency.
   - **Alternative Hypothesis:** The hypothesis stating that the null hypothesis is false, or an alternative view about the origin of the data.
      - Can be more vague than the null hypothesis.
   - **Test Statistic:** What the simulation is trying to predict/observe about the distributions
      - For a scenario:(Null: coin is fair)(Alternative:...):
      - if the alternative is "coin is biased towards heads or tails", then only recording the number of heads or tails in a sample is sufficient as a statistic.
      - if the alternative is "coin is not fair", then recording the (simulated heads or tails - expected number of heads or tails) is the best statistic.
   - **Questions** What values of the test statistic will make us lean towards the null? Alternative?
      - Prefer to choose "just high" and "just low" rather than "high or low values" of the statistic to lean towards the null/alternative.
   - **Hypothesis Testing:** Determines which hypothesis is better supported by the data. 
      - You must have one hypothesis that is simulatable by data
   - **Empirical Distribution Under the Null:** The distribution seen when simulating the test statistic under the assumptions of the null hypothesis. Can be done for any statistic.     
      - Shows likely values of the statistic and their probabilities of occurring (**approximate** because we can't generate all possible random samples)
   - **Conclusion** resolves the choice between null and alternative hypothesis
      - **Reject the null hypothesis** if the observed value(s) are inconsistent with the distribution of the null. Also can say "Is not consistent with the Null"
   - **Tails:** If the test statistic is in the tail of the empirical distribution and the area of the tail is less than 5%, then the result was statistically significant.
      - If it is less than 1%, then the result was HIGHLY statistically significant.
   - **P-Value (Observed Significance Level):** The chance under the null hypothesis that the test statistic is equal to the observed value or takes on values favoring the alternative
      - Calculate as `np.sum(simulated values [<= or >=] observed value)/total simulated values`
      -Small values of p support the alternative
         - The statistic is *not* the p-value. For some statistics, large statistic = small p-value (e.g. absolute value distance from 50% in coin flips if null is a fair coin).
         - Other statistics are the opposite- small statistic = small p-value (e.g. number of black men in panel when null is that there should be 20 black men)
      - Equal to the area under the curve **at or to the left/right/both of the observed value**
         - The alternative determines whether smaller or larger values of the statistic are included in the p-value calculations.
   - **Errors in Testing**
      - Test favors null, but the alternative is actually true!
         - Probability is equal to the cutoff value
      - Test favors alternative, but the null is actually true!
   - **P-value Cutoff**
      - Does not depend on observed data or simulation
      - Decided before simulation is run
      - Convention: 5% or 1%
         - Origin: 
   **Benford's Law:** P(first digit = d)= log(1+1/d)
      - Plotting the first numbers of the data should follow Benford's law. 
      - Useful for detecting fraud.

      