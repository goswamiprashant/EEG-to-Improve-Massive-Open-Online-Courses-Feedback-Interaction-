# EEG-to-Improve-Massive-Open-Online-Courses-Feedback-Interaction-
This is a Neurotech based Case study
In classroom education instant feedbacks can be taken from students like, whether  they understood the concept(lecture) or not ,but In online education there is big gap of Interaction between students and teachers ( MOOC Massive Open Online Courses that are free online courses available for anyone to enroll ).Here with the help of EEG we will record electrical activity of students’ brain at the time of watching online course material and try to predict whether the student is confused or not.. It is to be compared with the performance to human observers observing body language in predicting students’ confusion.This pilot study shows promise for MOOC-deployable EEG devices being able to capture tutor relevant information.
 
Dataset  :https://www.kaggle.com/wanghaohan/confused-eeg

Research-Papers/Solutions/Architectures/Kernels
Confused or Not Confused ?
Disentangling Brain Activity from EEG Data Using Bidirectional LSTM Recurrent Neural Networks
Research paper Link: https://www.ncbi.nlm.nih.gov/pmc/articles/PMC5620019/

Overview
In classroom education instant feedbacks can be taken from students like, whether  they understood the concept(lecture) or not ,but In online education there is big gap of Interaction between students and teachers ( MOOC Massive Open Online Courses that are free online courses available for anyone to enroll ).Here with the help of EEG we will record electrical activity of students’ brain at the time of watching online course material and try to predict whether the student is confused or not.. It is to be compared with the performance to human observers observing body language in predicting students’ confusion.This pilot study shows promise for MOOC-deployable EEG devices being able to capture tutor relevant information.
 


Experimental design
In this  pilot study, we collected EEG signal data of college students while they watched MOOC video clips. We extracted online education videos that are assumed to be not confusing for a college student, like videos of introduction of basic algebra or geometry. We also prepare videos that are assumed to confuse a normal college student if a student is not familiar with the video topics like Quantum Mechanics, Stem Cell Research 1 . We prepared 20 videos, 10 in each category. Each video was about 2 minutes. We chopped the two-minute clip in the middle of a topic to make the videos more confusing.After each session, the student rated his/her confusion level on a scale of 1-7, where 1 corresponded to the least confusing and 7 corresponded to the most confusing,we  also have predefined confusion level for all those videos.
So the classifier is build on top of student rated confusion level and also on predefined level. 

Features 


Features	Description	Sampling rate	Statistic

Attention	Proprietary measure of mental focus	1 Hz	Mean
Meditation	Proprietary measure of calmness	1 Hz	Mean
Raw	Raw EEG signal	512 Hz	Mean
Delta	1–3 Hz of power spectrum	8 Hz	Mean
Theta	4–7 Hz of power spectrum	8 Hz	Mean
Alpha1	Lower 8–11 Hz of power spectrum	8 Hz	Mean
Alpha 2	Higher 8–11 Hz of power spectrum	8 Hz	Mean
Beta1	Lower 12–29 Hz of power spectrum	8 Hz	Mean
Beta 2	Higher 12–29 Hz of power spectrum	8 Hz	Mean
Gamma1	Lower 30–100 Hz of power spectrum	8 Hz	Mean
Gamma2	Higher 30–100 Hz of power spectrum	8 Hz	Mean


The following features are values in different frequency regions of the power spectrum. The sampling frequency for the features extracted from the MindSet is 2 Hz. For each sample point, there are 14 features extracted from EEG signals.

For each subject  features are extracted at a sampling frequency of 2 Hz. The final features are truncated to around one-minute long. (As a video  is 2 min long so initial 30 sec  and final 30 sec are removed )So there are around 120 sample features for each data point. For models the only accept fixed-length features, we took the minimum number of samples (112 samples at last) and concatenated all the time steps to create a single feature vector. For the LSTM, the length of the time-series data is 112, each with a 12-dimension feature. In the end there are 100 data points, each with 112 × 12 features in total.

RNN-LSTM and BI-DIRECTIONAL LSTM
In RNN exploding gradients can be mitigated via truncation or squashing(because of activation functions. Vanishing gradients are more difficult to fix. The Long Short-Term Memory RNN (LSTM) addresses this problem by introducing memory units to RNNs. 
While LSTMs predict the current output based on previous inputs, bidirectional LSTMs predict the current output based on both the past and the future. The basic idea is to predict the future based on the past and predict the past based on the future, then take the average of these two outputs as the finial output. In this way both future and past context information can be utilized to improve performance.
Batch Normalization also plays important role in keeping the distribution of  input  same over multiple layers.
Result

Average accuracy for 5 fold cross validation

     Classification method          Accuracy(%)
     
       SVM (Linear kernel)             67.2
            
       SVM  (RBF Kernel)              51.3

       SVM (Sigmoid Kernel)         51.0

         KNN                                   51.9

        CNN                                    64.0

      RNN-LSTM                           69.0

       Bi-LSTM                              73.3
The accuracy across the five folds of the RNN-LSTM model varies from 60 percent to 85 percent, while that of the Bidirectional LSTM model varies from 71 percent to 74 percent. These results show that the Bidirectional LSTM model not only outperforms all the other methods, but also is consistent.

First Cut Approach
I have performed  EDA on the dataset based on those conclusions,I will prefer to start with Random Forest and Kernel  SVM because Individually features are not contributing enough ,and  bunch of features are co-related to each other significantly so decision tree as a base learner in Random forest,  it will leverage the essence of feature interaction and also look after the variance of the feature,because value of different features ranges from (10^-1 to 10^6).  As it is a low dimensional data, so  SVM (kernel)  implicitly maps their inputs to  higher dimensional feature space and it can perform better  in high dimension as combination of features seems significant here ,that is my intuition and that's how I want to proceed.

 EDA on Confused EED dataset : https://colab.research.google.com/drive/1hx_cErpQwJuJlnzu8i_Tl5jiW6oImhx2?usp=sharing
 
