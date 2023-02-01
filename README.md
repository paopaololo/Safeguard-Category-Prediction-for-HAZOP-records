# Safeguard Category Prediction for HAZOP records
This is the code I used to identify the safeguard categories in a Safety Risk Analysis Workshop, such as HAZOP and LOPA workshops. 
This readme only aims to give the reader a brief introductions of:
1. The background for the applied field - functional safety.
2. The Naive Bayes algorithm


# HAZOP and LOPA
Hazard and Operability Study (HAZOP) and Layers of Protection Study (LOPA) workshops are the main parts of the first phase of the IEC 61508 and IEC 61511 Functional Safety Management Lifecycle. The objective of these two activities are:
- to investigate and predict all the specific hazards within a industrial plant;
- to find out the existing safeguards in the design;
- to assess the risk gap to prevent hazards.

![image](https://user-images.githubusercontent.com/107201347/212220837-bb79a9d3-b008-4b88-b4be-082767fb6b85.png)
*An example of HAZOP record. from https://www.consiltant.com/en/process-safety/hazop/*

# Safeguards
The safeguards identified in the study need to be passed to different departments for detailed design and deployment. 
The most common safeguards in the industry are "Alarms", "Basic Process Control System Interlocks", "Relief devices", "Safety Instrumented Functions" and "Mechanical Integrity".
- **Alarms** aim to alert operators of an abnormal situation and required operator manual intervention. Alarms are usually the first defence in a safety incident. 
- **Basic Process Control System(BPCS) Interlocks** are automatic functions that exist in the BPCS system. They function to exert automatic control of the chemical process. An example would be the autostart of a sump pump.
- **Relief Devices** are typically relief valves and rupture discs. They are set to rupture or open at a certain pressure to release pressure from the enclosed process.
- **Safety Instrumented Functions (SIFs)** are automatic functions that exist in the Safety Instrumented System (SIS). They function like interlocks, but they are designed for specific Hazards. All SIFs need to have their own evaluated Safety Integrity Level (SIL) per the IEC 61508. An example would be shutting off the fuel gas supply to the furnace if the temperature is too high.
![image](https://user-images.githubusercontent.com/107201347/212571751-4c33b8b9-e340-4a0a-9146-df5fabc7b13a.png)
*The hardware structure of a SIF.  Copyright © exida.com*

- **Mechanical Integrity** is usually referring physical containments. An example would be the dike around a process vessel.

# Safeguard Categories
![image](https://user-images.githubusercontent.com/107201347/212577687-32ddbe65-ee6e-4bb0-bc2b-f82f48854b37.png)

*Initial phase of the IEC 61508 Lifecycle.  Copyright © exida.com*

Risk Analysis workshops like HAZOP mark the beginning of the lifecycle in the IEC 61508. Its outputs provide valuable information for the design of various systems, i.e. SIS, BPCS, physical layouts, and Alarm systems. Therefore, by identifying the categories of the safeguards, we can direct the right information to the corresponding departments for detailed designs.

Usually, HAZOP recording software does not have the field to capture the safeguard category, which instills obstacles for the autonomous process that channels the information to the right department for in-depth designs. I am using the Naive Bayes Classifier to predict the safeguard categories, which is based on the safeguard description in the HAZOP records.

# Naive Bayes Classifier
The Naive Bayes Classifier is a probabilistic classifier based on the Bayes Theorem:

![image](https://user-images.githubusercontent.com/107201347/212579607-fb2ba467-eec0-4fb8-a313-e5061b0372e8.png)

>Where A and B are events, and P(B) is not equal to 0. 

In this use case, I am trying to predict multiple categories ('Instrumented Protection Function', 'Alarm and Operator Intervention', 'Redundant Backup Equipment / System', 'Basic Process Control System', 'Check Valve'). Hence, I opt for the multinomial event model:

![image](https://user-images.githubusercontent.com/107201347/212580981-0f9747e2-4698-4f34-8773-c4881915ca5a.png)

>for each of *K* outcomes (classes *Ck*), and *X* vector (containing n features) 

If we express the probabilistic model in the log-space, we can turn it into a linear classifier, which simplifies the regression model.

The Native Bayes Classifier uses each word in the safeguard description and passes that "Bag of words" into the calculation model to predict the "classes" of the safeguard.

## Naive Bayes assumptions and why it is suitable for this use case 
The "Naive" in the Naive Bayes Classifier is naive because it naively assumes all inputs for the model are **independent** of each other. In the context of a textual classifier, it is highly inaccurate. 

For example, consider "High temperature leading to melting polar ice." If we vectorize the whole sentence based on each word, the Naive Bayes Classifier will not read through the whole sentence and realize the grammatical function of "High" as an adjective modifying the noun "temperature". To the algorithm, they are two separate words, without any combined meaning of "Temperature being high" or simply "Hot". 

Another assumption of the Naive Bayes Classifier is the **equal weight** of each input. In our example "High temperature leading to melting polar ice.", the algorithm will give the same importance to the words "high" and "to", even though "to" in this clause is just a preposition.

Even though Naive Bayes assumptions are not ideal for textual classifications, it is efficient in our use case for the following reasons: 
1. The languages used in HAZOP are highly standardized. IEC 61882 is the international standard that dictates the proper conducting of HAZOP. The "bag of words" passed into the classifier is more or less the same across the oil and gas industry. 
2. The grammatical structures of the safeguard descriptions are mostly phrases or clauses, instead of full sentences. This reduces the interdependency for each word and another.

# Structure of the algorithm
This section outlines a brief workflow of the algorithm. Please check the jupyter code for details.

The algorithm follows the workflow below:
1. Inputting the dataset
2. Data cleaning
   - Transforming safeguard description by removing redundant information and stop words, lowercasing and lemmatizing.
     - These can reduce the uneven weights of the different words in the description.  
   - Transforming and removing outliers in the safeguard category column.
   - Ordinal encoding of the Categories.
3. Splitting the dataframe into training and test data
4. Multinomial Naive Bayes Algorithm
5. Testing the model with new data
6. Creating Word Clouds for visualization
