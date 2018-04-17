# Peripheral Arterial Disease
## Project description

A clinical research team is planning a retrospective study of peripheral arterial disease (PAD) to evaluate comorbidities (i.e., medical conditions that can occur along with PAD) and risk factors associated with PAD. An analyst is recruited to help build a cohort of patients with the disease. The research team requested that there are a few important variables that are indicative of a PAD diagnosis: explicit mention of a PAD diagnosis in a clinical note and specific values of a test called the Ankle Brachial Index (ABI) that are also recorded in notes.

The task was to develop a system to identify positive mentions of PAD from each document in a set of clinical notes.

Clinical context also has temporality and experiencer axes. However, peripheral arterial disease is a chronic condition that develops over time, therefore, it is never mentioned in hypotherical context (*"if PAD develops..."). It also never truly goes way, so if a patient had it befor, he/she still has the disease. Therefore, temporality axis always has value "Current". The disease also is never mentioned in relation to someone other than the patient, so the experiencer axis is always "Patient". Thus, the only context axis that matters is "Negation" with values of either "Affirmed" or "Negated".

The expected output is a comma-delimited file that would have the following columns:

 - patient identifier,
 - document identifier,
 - span starting index,
 - span ending index,
 - span snippet (the PAD mention string itself, e.g., "peripheral artery disease"),
 - affirmed/negated flag (e.g., an affirmative example: "Mr. Jones has periperhal artery disease." so flag should indicate an affirmed mention; a negative example: "Tests indicate no PAD present." so flag should indicate this is a false mention of PAD)

## The following outlines the steps required to perform the project

### Create reference standard

NLP system development requires manual annotation. [Classification_prep.ipynb](Classification_prep.ipynb) outlines the steps to create document sets for training and testing a system. Splitting the set into training and testing can be done either before annotation or after. The example in Classification_prep notebook shows splitting the set before manual annotaiton.
2 Implement the system

The task falls into category of mention classification tasks which have the following general steps:

    Identifying the concept
    Building context window
    Classifying the concept based on context

The same general design can be implemented in multiple ways.

    Simple_Classification_System.ipynb -- uses regular expressions to find mentions of the target concept and checks for negative modifiers up to 30 characters before the target mention.

Will be shared by request with point penalty:

    Classification_Rule_Based.ipynb -- uses the system built in Module 5 with a slight modification using the wrapperPyConText.convertMarkupsAnnotations function to convert PyConText annotations to pipeUtils.Document annotations.

    Classification_System.ipynb -- uses wrapperPyConText to encapsulate pyConTextNLP code and adapts it to the framework defined in pipeUtils.

3 Validate the System

Simple_Classification_System.ipynb notebook defines the code to read the test files using the functionality in pipeUtils framework, apply the NLP system on the documents, and produce efficacy metrics for each document and aggregate for the whole set.
4 Deploy the system

MIMIC II contains a small enough dataset that can be processed at once. Simple_Classification_System.ipynb notebook shows how to output the results of the classification system by writing them to a csv file. The easiest way to output is using Pandas dataframe to_csv method. The dataframe and the output file can be evaluated to determine the total number of records produced by the system.
