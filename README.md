# Safeguard-Category-Prediction-for-HAZOP-records
This is a project I used to identify the safeguard categories in a Safety Review Workshop, such as HAZOP and LOPA workshops. 
This readme only aims to give the reader a brief introductions of:
1. The background for the applied field - functional safety.
2. The Naive Bayes algorithm


# HAZOP and LOPA
HAZOP and LOPA workshops are the main parts of the first phase of the IEC 61508 and IEC 61511 Functional Safety Management Lifecycle. The objective of these two activities are:
- to investigate and predict all the specific hazards within a industrial plant;
- to find out the existing safeguards in the design;
- to assess the risk gap to prevent hazards.

![image](https://user-images.githubusercontent.com/107201347/212220837-bb79a9d3-b008-4b88-b4be-082767fb6b85.png)
*A example of HAZOP record. from https://www.consiltant.com/en/process-safety/hazop/*

# Safeguards
The safeguards identified in the study need to be passed to different departments for detailed design and deployment. 
The most common safeguards in the industry are "Alarms", "Basic Process Control System Interlocks", "Relief devices", "Safety Instrumented Functions" and "Mechanical Integrity".
- **Alarms** aim to alert operators of an adnormal situation and required operator manual intervention. Alarms are usually the first defence in a safety incident. 
- **Basic Process Control System(BPCS) Interlocks** are automatic functions that exist in the BPCS system. They function to exert automatic control of the chemical process. An example would be the autostart of a sump pump.
- **Relief Devices** are typically relief valves and rupture discs. They are set to rupture or open at a certain pressure to release pressure from the enclosed process.
- **Safety Instrumented Functions (SIFs)** are automatic functions that exist in the Safety Instrumented System (SIS). They function like interlocks, but they are designed for specific Hazards. All SIFs need to have their own evaluated Safety Integrity Level (SIL) per the IEC 61508. An example would be shutting off the fuel gas supply to the furnace if the temperature is too high.
![image](https://user-images.githubusercontent.com/107201347/212571751-4c33b8b9-e340-4a0a-9146-df5fabc7b13a.png)
*The hardware structure of a SIF.  Copyright © exida.com*

- **Mechanical Integrity** are usually referring physical containments. An example would be the dike around a process vessel.

# Safeguard Categories
![image](https://user-images.githubusercontent.com/107201347/212577687-32ddbe65-ee6e-4bb0-bc2b-f82f48854b37.png)
Risk Analysis workshops like HAZOP marks the beginning of the lifecycle in the IEC 61508. Its outputs provide valuable information for the design of various systems, i.e. SIS, BPCS, physical layouts, and Alarm systems. Therefore, by identifying the categories of the safeguards, we can direct the right information to the corresponding departments for detailed designs.

Usually, HAZOP recording software does not have the field to capture the safeguard category, which instills obstacles for the autonomous process that channels the information to the right department for in-depth designs. I am using the Naive Bayes Classifier to predict the safeguard categories based on the safeguard description in the HAZOP records.

# Naive Bayes Classifier
The Naive Bayes Classifier is a probabilistic classifier based on the Bayes Theorem:

![image](https://user-images.githubusercontent.com/107201347/212579607-fb2ba467-eec0-4fb8-a313-e5061b0372e8.png)

>Where A and B are events, and P(B) is not equal to 0. 

In this use case, I am trying to predict multiple categories ('Instrumented Protection Function', 'Alarm and Operator Intervention', 'Redundant Backup Equipment / System', 'Basic Process Control System', 'Check Valve'). Hence, I opt for the multinomial event model:

![image](https://user-images.githubusercontent.com/107201347/212580981-0f9747e2-4698-4f34-8773-c4881915ca5a.png)

>for each of *K* outcomes (classes *Ck*), and *X* vector (containing n features) 

If we express the probabilistic model in the log-space, we can turn it into a linear classifier, which simplifies the regression model.

## Naive Bayes assumption and why it is suitable for this use case 



