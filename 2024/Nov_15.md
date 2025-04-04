# Weekly Report

![Web Search Agent_Nov13](https://raw.githubusercontent.com/ChengAoShen/Image-Hosting/main/images/Web%20Search%20Agent_Nov13.svg)

## Newest Experiment Report(GPT4o-mini)

**Auto_MPG**

|                           Methods                            | FPR  | FNR  | Precision |  F1  | SHD  | NHD  |
| :----------------------------------------------------------: | :--: | :--: | :-------: | :--: | :--: | :--: |
|                         Baseline(PC)                         | 0.4  | 0.8  |   0.11    | 0.14 |  8   | 0.32 |
|                     PC + Constrain Agent                     | 0.15 | 0.2  |   0.57    | 0.66 |  3   | 0.12 |
| **PC + Constrain Agent + Information Supplement Agent(Ours)** | 0.0  | 0.2  |    1.0    | 0.88 |  1   | 0.04 |
|          PC + Constrain Agent + Fixed format search          | 0.1  | 0.2  |   0.66    | 0.72 |  2   | 0.08 |
|                       PC + React Agent                       | 0.15 | 0.4  |    0.5    | 0.54 |  4   | 0.16 |
|                 PC + React Constrain Agents                  | 0.15 | 0.2  |   0.57    | 0.66 |  3   | 0.12 |
|                      MAC(Tinghua paper)                      |      |      |           |      |  4   | 0.16 |

**DWD_Climate**

|                           Methods                            | FPR  | FNR  | Precision |  F1  | SHD  | NHD  |
| :----------------------------------------------------------: | :--: | :--: | :-------: | :--: | :--: | :--: |
|                         Baseline(PC)                         | 0.2  | 0.83 |   0.14    | 0.15 |  9   | 0.25 |
|                     PC + Constrain Agent                     | 0.06 | 0.66 |    0.5    | 0.4  |  6   | 0.16 |
| **PC + Constrain Agent + Information Supplement Agent(Ours)** | 0.03 | 0.5  |   0.75    | 0.88 |  4   | 0.11 |
|          PC + Constrain Agent + Fixed format search          | 0.03 | 0.66 |   0.66    | 0.44 |  5   | 0.13 |
|                       PC + React Agent                       | 0.06 | 0.66 |    0.6    | 0.4  |  6   | 0.16 |
|                  PC + React Constrain Agent                  | 0.03 | 0.5  |   0.75    | 0.6  |  4   | 0.11 |
|                      MAC(Tinghua paper)                      |      |      |           |      |  6   | 0.16 |

**Sachs**

|                           Methods                            | FPR  | FNR  | Precision |  F1   | SHD  | NHD  |
| :----------------------------------------------------------: | :--: | :--: | :-------: | :---: | :--: | :--: |
|                         Baseline(PC)                         | 0.15 | 0.47 |   0.38    | 0.44  |  24  | 0.19 |
|                     PC + Constrain Agent                     | 0.19 | 0.84 |   0.13    | 0.14  |  27  | 0.22 |
| **PC + Constrain Agent + Information Supplement Agent(Ours)** | 0.04 | 0.84 |   0.38    | 0.22  |  17  | 0.14 |
|          PC + Constrain Agent + Fixed format search          | 0.04 | 0.84 |   0.38    | 0.22  |  17  | 0.14 |
|                       PC + React Agent                       | 0.20 | 0.94 |   0.04    | 0.048 |  29  | 0.23 |
|                  PC + React Constrain Agent                  | 0.20 | 0.89 |   0.08    | 0.095 |  30  | 0.24 |
|                      MAC(Tinghua paper)                      |      |      |           |       |  21  | 0.19 |

## Demo of Auto_MPG

### Information dataset collection

**Web Search Question**: what is the auto_mpg dataset, and what are the variables displacement, mpg, horsepower, weight, and acceleration?

> **Answer**: the auto_mpg dataset is a well-known dataset in the field of machine learning and statistics, primarily used for regression analysis and predictive modeling. it contains information about various automobiles, specifically focusing on their fuel efficiency measured in miles per gallon (mpg). the dataset includes several variables that are crucial for understanding vehicle performance, including displacement, mpg, horsepower, weight, and acceleration.
>
> 1. **displacement**: this variable refers to the total volume of all the cylinders in an engine, typically measured in cubic inches or liters. it is a key indicator of engine size and capacity. generally, a larger displacement can lead to more power output, but it may also result in lower fuel efficiency.
>
> 2. **mpg (miles per gallon)**: this is the target variable in the dataset, representing the fuel efficiency of the vehicle. higher mpg values indicate better fuel efficiency, meaning the vehicle can travel further on a gallon of fuel. the relationship between mpg and other variables is often inverse; as engine size (displacement) and weight increase, mpg tends to decrease.
>
> 3. **horsepower**: this variable measures the power output of the engine. higher horsepower typically correlates with better acceleration and overall performance. however, like displacement, higher horsepower can also lead to lower fuel efficiency, as more power often requires more fuel.
>
> 4. **weight**: the weight of the vehicle is another critical factor affecting performance. heavier vehicles generally require more power to accelerate, which can negatively impact mpg. there is often a negative correlation between weight and mpg; as weight increases, fuel efficiency tends to decrease.
>
> 5. **acceleration**: this variable measures how quickly a vehicle can increase its speed, usually defined as the time taken to go from 0 to 60 mph. Acceleration is influenced by both horsepower and weight; more powerful engines can accelerate faster, while heavier vehicles may struggle to achieve the same acceleration.
>
> Understanding these relationships is crucial for analyzing vehicle performance and making informed decisions regarding vehicle design and efficiency improvements.
>
> **References**
>
> [1] URL: https://archive.ics.uci.edu/ml/datasets/auto+mpg  
> Quoted sentence: "The dataset contains 398 instances and 8 attributes, including MPG, cylinders, displacement, horsepower, weight, acceleration, model year, and origin."

**Web Search Question**: What are the relationships between its variables such as Displacement, MPG, Horsepower, Weight, and Acceleration?

> Answer: The Auto_MPG dataset is a well-known dataset that contains information about various attributes of automobiles and their fuel consumption, specifically measured in miles per gallon (MPG). The dataset includes five key continuous variables: **Weight**, **Displacement**, **Horsepower**, **Acceleration**, and **MPG**. It is often used in data analysis and machine learning to understand the factors that influence fuel efficiency in cars.
>
> **Relationships Between Variables**
>
> 1. **Displacement**: Displacement refers to the engine size of the car, and it has been found to have a strong negative correlation with MPG. This means that as the displacement increases, the MPG tends to decrease. This relationship is intuitive, as larger engines typically consume more fuel.
>
> 2. **Horsepower**: Similar to displacement, horsepower also shows a negative causal effect on MPG. Higher horsepower engines are generally less fuel-efficient, which is supported by statistical analysis indicating a significant negative relationship between horsepower and MPG [3].
>
> 3. **Weight**: The weight of the car is another critical factor affecting fuel efficiency. Heavier cars tend to have lower MPG values, as more energy is required to move a heavier vehicle. This relationship is also confirmed through causal inference results, indicating that increasing the weight of a car significantly reduces its fuel efficiency [3].
>
> 4. **Acceleration**: Acceleration has a complex relationship with MPG. While faster acceleration can be associated with lower fuel efficiency, the analysis suggests that the relationship is not as straightforward as with displacement and horsepower. The causal effect of acceleration on MPG is negative, indicating that cars designed for quicker acceleration may sacrifice fuel efficiency [3].
>
> 5. **Model Year**: The model year of a car has a positive correlation with MPG, suggesting that newer vehicles tend to be more fuel-efficient. This relationship is statistically significant, indicating that advancements in technology and design in newer models contribute to better fuel efficiency [3].
>
> **Summary of Findings**
>
> - **Negative Correlations**: Displacement, horsepower, and weight are negatively correlated with MPG, meaning that increases in these variables lead to decreases in fuel efficiency.
> - **Positive Correlation**: The model year shows a positive correlation with MPG, indicating that newer cars are generally more fuel-efficient.
> - **Complex Relationships**: Acceleration has a negative effect on MPG, but the relationship is influenced by other factors and may not be as direct as with weight or displacement.
>
> In conclusion, the Auto_MPG dataset provides valuable insights into how various automobile attributes affect fuel consumption. Understanding these relationships can help in designing more fuel-efficient vehicles and improving overall automotive performance.
>
> **References**
>
> [1] URL: https://arxiv.org/html/2407.15073  
> Quoted sentence: "This dataset consists of the variables around the fuel consumption of cars. With five variables: “Weight”, “Displacement”, “Horsepower”, “Acceleration” and “Mpg”(miles per gallon)"
>
> [2] URL: https://medium.com/@kavyamedikonda823/auto-mpg-analysis-bea552fd6e12  
> Quoted sentence: "From the overall analysis displacement, weight and horse power are strongly negatively correlated with mpg."
>
> [3] URL: https://python.plainenglish.io/exploring-the-causal-effects-of-automobile-attributes-on-mpg-a-data-driven-approach-fbd5f0fb294a  
> Quoted sentence: "Horsepower shows a negative causal effect on MPG, suggesting that more powerful engines may reduce fuel efficiency."

Summary:

```
Dataset Summary: The Auto_MPG dataset is a well-known dataset in the field of machine learning and statistics, primarily used for regression analysis and predictive modeling. It contains information about various automobiles, specifically focusing on their fuel efficiency measured in miles per gallon (MPG). The dataset includes several variables that are crucial for understanding vehicle performance, including Displacement, Mpg, Horsepower, Weight, and Acceleration.

---
Summary of the Displacement: Displacement refers to the total volume of all the cylinders in an engine, typically measured in cubic inches or liters. It is a key indicator of engine size and capacity. Generally, a larger displacement can lead to more power output, but it may also result in lower fuel efficiency.

---
Summary of the Mpg: Mpg (Miles per Gallon) is the target variable in the dataset, representing the fuel efficiency of the vehicle. Higher MPG values indicate better fuel efficiency, meaning the vehicle can travel further on a gallon of fuel. The relationship between MPG and other variables is often inverse; as engine size (Displacement) and weight increase, MPG tends to decrease.

---
Summary of the Horsepower: Horsepower measures the power output of the engine. Higher horsepower typically correlates with better acceleration and overall performance. However, like displacement, higher horsepower can also lead to lower fuel efficiency, as more power often requires more fuel.

---
Summary of the Weight: The weight of the vehicle is another critical factor affecting performance. Heavier vehicles generally require more power to accelerate, which can negatively impact MPG. There is often a negative correlation between weight and MPG; as weight increases, fuel efficiency tends to decrease.

---
Summary of the Acceleration: Acceleration measures how quickly a vehicle can increase its speed. It is influenced by both horsepower and weight; more powerful engines can accelerate faster, while heavier vehicles may struggle to achieve the same acceleration.
```

## Problems

DYNOTEAR SP100 dataset is too large and has no ground truth.

Dream4 has Ground truth, but is too large



