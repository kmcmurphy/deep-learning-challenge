# Deep Learning Challenge
In this progject, deep learning via TensorFlow was used to analyze a CSV file with over 34,000 lines and through various features in the data, create a model that would help predict if an applicant would be successful if chosen to be funded by the Alphabet Company. An initial model was tested and then further optimazations were completed to see if the initial model could be improved upon.

## Data Preprocessing
- The first step was to drop non-categorical data, so the NAME and EIN columns were removed from the data
- Next, columns with a high number of unique values were reduced into broader bins
  - APPLICATION_TYPE had all values with a count less than 528 put into an "Other" category
  - CLASSIFICATION counts less than 1883 were reclassified as "Other" 
- For our X, there were two columns that were already binary, STATUS and IS_SUCCESSFUL
  - Looking at the value counts for each, IS_SUCCESSFUL represented a more even distribution of values which would lend itself better for training
 
The data was then one-hot encoded using the Pandas get_dummies methodf and split into training and testing data.

## The Initial Model
For our initial learning model, only one hidden layer was used with neuron counts of 80 and 30 for our input and hidden layer. The relu activation method was used.

The model produce an accuracy of 72% with a loss of 56%

![download](https://github.com/kmcmurphy/deep-learning-challenge/assets/7529880/ca34ab4a-9a9a-4d54-93d2-a060ccc912ec)

## Optimizing the Model
The first model was far from perfect, so further optimization would be needed. In the end, many methods were attempted before choosing three different sets of parameters to train the data on. Most of the initial optimzation steps were abandoned after producing no discernable improvement in the model.

<b>Optimization steps attempted</b>
  - The first step taken (and kept) was to reduce extra noise in the data. This meant dropping STATUS and SPECIAL_CONSIDERATIONS. Neither of these features were diverse enough in count.
  - Further bin the original columns
    - APPLICATION_TYPE and CLASSIFICATION were adjusted so that each would only have four unique values
    - This was later abandoned
   - Bin the ASK_AMT
    - One column that still had a number of unique  values that could be converted to features was ASK_AMT
    - The first attempt was to bin the amounts into six buckets
    - The second attempt was to reduce this to four buckets
    - Neither produced a significant improvement
  - Along with changing the data, Keras Tuner was used to try and find the optimal settings
    - The count of neurons and hidden layers were bounced against the relu, tanh and sigmoid activation methods
    - Epochs were set ranging from 4 to 100
  
  Overall, the highest accuracy obtained was <u><b>74%</b></u>.

## Settling on Models
In the end, I settled on three approaches for training the data.

- The First Model
  - Activation method: relu
  - Total layers: 5
  - Nueron structure: 100/100/75/50/225
  - Overall accuracy: 72.6% with a loss of 56.65%

![download](https://github.com/kmcmurphy/deep-learning-challenge/blob/main/images/optimized_trial_2.png)

- The Second Model
  - Activation method: tanh
  - Total layers: 5
  - Nueron structure: 100/100/75/50/225
  - Overall accuracy: 72.7% with a loss of 56.9%

![download](https://github.com/kmcmurphy/deep-learning-challenge/blob/main/images/optimized_trial_1.png)

- The Third Model
  - Activation method: relu
  - Total layers: 5
  - Nueron structure: 100/2000/1000/500/225
  - Overall accuracy: 73.13% with a loss of 61.9%

![download](https://github.com/kmcmurphy/deep-learning-challenge/blob/main/images/optimized_trial_3.png)

Based on these results, I would re-examine the initial name column removed, as there were many duplicates in the values, which if binned correctly, may have provided more features to examine and learn from. I also would have done additional research on the parameters available in Keras tuner that might have optimized its testing and analysis of an optimal model format.
