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

## Tools for Making Inferences

- **Null Hypothesis:** A well defined chance model about how the data was generated. We can simulate data *under the null hypothesis*.
      - Simulate under the null hypothesis will give and emperical distribution of the statistic under the null hypothesis. If the observed data is not consistent with the distribution, then the alternative is favored.
      - Use a visualization to verify consistency.
- **Alternative Hypothesis:** The hypothesis stating that the null hypothesis is false, or an alternative view about the origin of the data.
      - Can be more vague than the null hypothesis.
- **Test Statistic:** What the simulation is trying to predict/observe about the distributions
   - For a scenario:(Null: coin is fair)(Alternative:...):
   - if the alternative is "coin is biased towards heads or tails", then only recording the number of heads or tails in a sample is sufficient as a statistic.
   - if the alternative is "coin is not fair", then recording the (simulated heads or tails - expected number of heads or tails) is the best statistic.
- **Questions we need to answer:** 
   - What values of the test statistic will make us lean towards the null? Alternative?
   - Prefer to choose "just high" and "just low" rather than "high or low values" of the statistic to lean towards the null/alternative.
- **P-Value (Observed Significance Level):** The chance under the null hypothesis that the test statistic is equal to the observed value or takes on values favoring the alternative
   - Calculate as `np.sum(simulated values [<= or >=] observed value)/total simulated values`
   - Small values of p support the alternative
      - The statistic is *not* the p-value. For some statistics, large statistic = small p-value (e.g. absolute value distance from 50% in coin flips if null is a fair coin).
      - Other statistics are the opposite- small statistic = small p-value (e.g. number of black men in panel when null is that there should be 20 black men)
   - Equal to the area under the curve **at or to the left/right/both of the observed value**
      - The alternative determines whether smaller or larger values of the statistic are included in the p-value calculations.
   - **P-value Cutoff**
      - Does not depend on observed data or simulation
      - Decided before simulation is run
      - Convention: 5% or 1%
- **Benford's Law:** P(first digit = d)= log(1+1/d)
  - Plotting the first numbers of the data should follow Benford's law. 
  - Useful for detecting fraud.
- **Conclusion** resolves the choice between null and alternative hypothesis
   - **Reject the null hypothesis** if the observed value(s) are inconsistent with the distribution of the null. Also can say "Is not consistent with the Null"

## Hypothesis Testing
   
   - **Purpose:** Determines which hypothesis is better supported by the data. 

   - **Methods of Hypothesis Testing**
      <!-- GO INTO PREVIEW MODE TO VIEW PROPERLY -->
      | Type of Data | Test Statistic | How to Simulate | Example |
      |---|---|---|---|
      | 1 Sample, 1 Category  |  empirical percentage  |  `sample_proportions(n,null_dist)` | Percentage of purple flowers  |
      |  1 Sample, Multiple Categories  | TVD(empircal_dist,null_dist)  | `sample_proportions(n,null_dist)`  |  Ethnicity distribution of jury panel |
      |  1 Sample, Numerical Data |  empirical mean | `data.sample(n,with_replacement = False)`  | Scores in section |
      | 2 Samples, Numerical Data  | difference in means  | `empirical_data.sample(with_replacement = False)`  |  Birth weight of smokers vs non smokers |
      - You must have one hypothesis that is simulatable by data
      
   - **Empirical Distribution Under the Null:** The distribution seen when simulating the test statistic under the assumptions of the null hypothesis. Can be done for any statistic.     
      - Shows likely values of the statistic and their probabilities of occurring (**approximate** because we can't generate all possible random samples)
   
   - **Tails:** If the test statistic is in the tail of the empirical distribution and the area of the tail is less than 5%, then the result was statistically significant.
      - If it is less than 1%, then the result was HIGHLY statistically significant.
    
   - **How to Simulate**
   ```python
      proportions = make_array(probability_true, probability_false)

      def one_statistic():
         return some_statistic_function(sample_proportions(num_in_sample, proportions).item(0))
      
      simulated_stats = make_array()

      for i in np.arange(num_simulations):
         simulated_stats = simulated_stats.append(simulated_stats, one_statistic())
   ```
   

## A/B Testing
      
   - **Purpose:** Testing to see if the difference in two groups is due to chance or not.
   
      - **Null**: The distributions  are the SAME
      - **Alternative**: example(In the population, the Group B, on average, has a lower/higher (x) than the Group A)
      - Statistic: (Group B(average) - Group A(average))
      - **Simulation**: 
         1. Define the null/alternate models
         2. Choose a test statistic (typically the one above)
         3. Find the value of the **observed** test statistic (based on your data)
         4. Shuffle the labels and find the simulated test statistic using the method below (repeat many times):
         ```python
         def select(table,label,group_label):
            reduced = table.select(label,group_label)
            means = reduced.group_by(group_label,np.average)
            return means.column(label average).take(1) - means.column(label average).take(0)
         ```
         5. Calculate the p-value by comparing the observed and simulated statistic
         6. Compare the p-value to the cutoff to draw a conclusion about the null hypothesis

         
   - **Permutation Test**
   - Shuffle around the labels and see if the average weights are the same as the original data. If they are similar, that means the labels have no association with the weights. Use `table.sample(n)`
      - Pick **without replacement** (`with_replacement=False`) because we don't want the same data point to be assigned to  different labels randomly
      - Use `table.sample(with_replacement=False)` with NO numerical input to get the same number of rows out as number of rows in the original table
      - Make new array as a shuffled column of the original labels shuffled by the table.sample() function and then add to the original.
         ```python
         def one_simulation(table,label,group_label)
            shuffled_labels = table.sample(with_replacement=False).column(label)
            original_and_shuffled = table.with_column('shuffled',shuffled_labels)
            return statistic(original_and_shuffled,label,'shuffled')     
         ```
         - Compare the distribution of this simulation to the observed difference (statistic) to find the p-value.
         - p = `np.count_nonzero(differences <less/greater than, etc> observed_difference) / len(differences)`

**Randomized Controlled Experiment**: 
   - Sample A: Control
   - Sample B: Treatment
      - If the treatment and control groups seem random, then you can make causual connections
      - Any difference in outcomes could be due to chance or the treatment.
      - For each person, they have two potential outcomes in a study.
         - The outcome if assigned  *treatment* and the outcome if assigned to *control* 
      - **Null:** The distribution of the *control* group's scores and the *treatment* group's scores are the same.
       (Treatment has no effect)
      - **Alternative:** The treatment scores are different/better/worse than the control
         - EX: compute the statistic(difference in scores(means) between the two groups) for randomizations of groups.

**Random Assignment Vs. Shuffling**

|Data Generation   |Sample Data   |Hypothesis Testing   |Conclusions|   |
|---|---|---|---|---|
|Observational Sample| Numerical Data with 2 Samples  |Shuffle labels to simulate from null   |Association   |   |
|Random Control Experiment   | Numerical Data with 2 Samples   |Shuffle labels to simulate from null   | Causation   |   |


## Quantifying Uncertainty

**Estimation:** If we want to get a value of a parameter for a population, a census is usually unrealistic. Therefore, estimation is a useful tool.
   - Use the table.sample(n,with_replacement=False) function to create a sample.
   - (estimate)= (true value) + (Error)

**Bootstrap:** A technique/process for simulating repeated random sampling
   - All we have is a large random sample
   - Then we resample from the sample (**with** replacement - allows variation in the resample rather than just being the same sample again!)
   - Used for getting many samples from only one sample of a population. Important because we almost never get census data
   - Very similar to population/real-world sampling if the sample is large enough
   - Resamples should be the **same size** as the original sample in order to properly evaluate variability and error
   - Bootstrap should not be used to:
      - estimate very high or low percentiles(max or min height)
      - estimate a parameter that is affected by rare elements
      - estimate for probability distributions that are not roughly normal
      - estimate from a very small original sample

**Confidence Intervals:** An x% confidence interval means that we are x% confident that the true value lies within a certain range, given estimates of a parameter
 - Confidence level: x% (0 <= x <= 100)
 - Higher confidence level = **wider** interval

 - **Generating confidence interval from bootstrap**
   1. Take a sample
   2. Use sample to make many bootstrap samples
   3. Construct a confidence interval based on the distribution of test statistics in the bootstrap samples
 ```python
 def construct_confidence_interval(confidence_level, population, n)
   """Given a confidence level, table with population data, and a number of samples to take, constructs a confidence interval based on bootstrap samples.
   """
   # ONLY if we are simulating and know the entire census data!!!
   our_sample = population.sample(n, with_replacement = False)
   # Get the median as a test statistic
   our_sample_median = percentile(50, our_sample.column('Data')) 
   # Create list of bootstrap sample statistics
   bootstrap_medians = np.array([one_bootstrap_median() for _ in np.arange(n)])

   # Example: If the confidence level was 95, the lower percentile would be 2.5 and the upper percentile would be 97.5
   left = percentile((100 - confidence_level)/2, bootstrap_medians) 
   right = percentile(100 - (100 - confidence_level)/2, bootstrap_medians)

   return [left, right]
```
- CI for testing:
   - Null: Population average = x
   - Alternative: Population average != x
   - Cutoff P = p%
   - Method:
      - Construct a 100-p% confidence interval for the population average
      - if x is not in the interval, reject null
      - if x is in the interval, can't reject the null

**Definitions of Center and Spread:**
- Mean: The weighted average of the data (Sum of point values/number of points)
   - Value depends on the distribution of the data, not the multiplicity of the distribution.
      - As long as the relative proportions and values of each data point is the same, multiplying everything by a number doesn't change the mean.
   - Not necessarily a value in the data
   - Somewhere between min and max
   - Same units as the data
   - "Center of mass"
- Median: Half-way point of the data
   - Value depends only on the the largest (or smallest) half of the data.
- Standard Deviation: Spread of the data around mean (sqrt( sum((value - mean)^2)/number of points - 1)
   - "root mean square of deviations from the average"
- Chebyshev's Inequality: for a constant z, (1 - 1/z^2)% of your data will be within (mean + or - z*standard deviation)
   - At least (1 - 1/5^2) = 96% of z-values will be between -5 and 5 standard deviations
   - NOT- inclusive of endpoints
- Z-scores aka standard units: a standard measure of how far from the average a value is 
   - (value - average) / standard deviation
   - z == 0:value equal to the mean
   - z > 0: value is above the mean
   - z < 0: value is below the mean
- When values are in standard units, the average is 0 and the SD is 1

**Normal Distributions:** Bell shaped distribution.
   - 68 / 95 / 99.7% of the data is within 1 / 2 / 3 standard deviations from the mean.

**Central limit theorem:**
   - Theorem which describes how the normal distribution is connected to sample averages.
   - **Conditions:**
      - Probability distribution of the sum or average of a sample
      - Sample must be large
      - Samples must be drawn with replacement
   - The larger the sample size, the closer to normal a distribution will become
   - Reasoning: From one sample, you have one average, but there is always a possibility that the sample average you got was different than another sample's average.
      - Imagine all possible random samples of the same size. 
      - Each has their own average.
      - The distribution of the sample average is the distribution of all possible samples.
      - For a population of N, the number of possible samples of sample size, n, are N^n
          - We don't need this many iterations- even a small fraction (10000) is enough to approximate the curve
   - Relation to Confidence Interval: 95% of all sample averages are within 2 standard deviations of the mean due to normality.
      - By association, the mean is within 2 standard deviations of 95% of points.
      
**Sample Proportions**
   - Proportions are **averages**
   - Simplified example: If a population is comprised of all true/false answers, then the proportion of Trues is the population average.
   - Sample SD = sqrt(p*(1-p))/n
   - If we want p% precision / margin of error
      - `Sample Size >= 4*SD/(p*.01)`
      - worst case: SD of the proportions = 0.5 for a proportion p = 0.5

**Sample SD and mean**
  As a sample size approaches the population size:
   - The sample mean approaches the population mean
   - The sample SD approaches the value `Population SD / n` where n is the sample size

**Classification and Regression:**
   - Classification is for **categorical** data; regression is for **numerical** data
   - Classifiers: Takes attributes of an example and outputs a prediction for the label.
   - *Evaluating the quality of a classifier:* Judge on precision and recall
      - Precision: The proportion of your predictions that are correct.(true positives/all positives)
      - Recall/True Positive Rate: Measures how many of the positive values were actually predicted correctly. (true positives/(predicted positives + false negatives))
      - Split sample into training set and test set and evaluate the accuracy of the prediction on the test set.
   - **Nearest Neighbor:** Used for classification. Basic idea: find the label of the entry in the training set that is most similar to the entry in question.
      - Ideal only for data where the distinction between the labeled data is clear.  
   - **Decision Boundary:** The line on which the prediction will change if a point is on one side vs the other
   - **k Nearest Neighbors:** Used for classification that is robust against the instance where there are 2 neighbors that are close in their features, but different in class.
      - to classify a point: find its k(odd number) nearest neighbors.
      - assign class to the point according to the class value with the greatest frequency among the k nearest neighbors.
   - **Measuring Association:** Quantifying the relationship between two variables with a number.
       - Convert to standard units using `(value - mean(value)) / SD`
       - Correlation Coefficient: number representing the *linear* association's direction and magnitude of the relationship. Represented by the letter 'r'. For standard units... 
         - Not really useful for anything other than linear relationships (make sure before computing)
         - Average of the product of x in standard units and y in standard units (see other page for code reference)
         - Positive: when x is positive, y is positive (in standard units)
         - Negative: when x is negative, y is negative (in standard units)
         - key: r = 1 means very positive, r = -1 means very negative, r = 0 means no relationship.
         - r = (standard devation of predictions)/(standard devation of actual values)
   - **Residuals:** Measures of error of an x_value relative to a prediction. Positive residuals represent underestimates, and negative residuals represent overestimates. res = (y_actual - y_pred)
      - Have a mean of zero 
      - Should have zero correlation with x and the y.
      - Should look like a formless blob when plotting residual vs. x_values
      - If we get a mean residual of *K* where k > 0, shift up the line by k to counteract the underestimation
      - RMSE = standard devaition of residuals
      - 

