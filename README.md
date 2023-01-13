# Safeguard-Category-Prediction-for-HAZOP-records
This is a project I used to identify the safeguard categories in a Safety Review Workshop, such as HAZOP and LOPA workshops. 
This readme only aims to give the reader a brief introductions of:
1. The background for the applied field - functional safety.
2. The Naive Bayes algorithm


# HAZOP and LOPA
HAZOP and LOPA workshops are the main parts of the first phase of the IEC 61508 and IEC 61511 lifecycle. The objective of these two activities are:
- to investgiate and predict all the specific harzards within a industrial plant;
- to find out the existing safegaurds in the design;
- to assess the risk gap to prevent the hazards.

![image](https://user-images.githubusercontent.com/107201347/212220837-bb79a9d3-b008-4b88-b4be-082767fb6b85.png)
*A example of HAZOP record. from https://www.consiltant.com/en/process-safety/hazop/*

## Safeguards
The safeguards identified in the study need to be passed into different departments for detailed design and deployment. 
The most common safegaurds in the industry are "Alarms", "Basic Process Control System Interlocks", "Relief devices", "Safety Instrumentted Functions" and "Passive preventions".
- **Alarms** aims to alert operator of adnormal situation and required operator manual intervention. Alarms are usually the first defence in a safety incident. 
- **Basic Process Control System(BPCS) Interlocks** are automatic functions exist in the BPCS system. They function to exert automatic control of the chemical process. An example would be the autostart of a sump pump.
- **Relief Devices** 


