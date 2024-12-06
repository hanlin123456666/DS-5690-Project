# DS-5690-Project
# Utilizing Whisper for Identifying Cleft-Related Hypernasality in Patient Voice Samples
Hanlin Chen

hanlin.chen@vanderbilt.edu

## Introduction
Cleft palate is a craniofacial disorder that affects roughly 1 in 700 live births worldwide and is typically addressed through surgical intervention [1]. However, even after corrective surgery, approximately 30% of patients continue to experience speech deficiencies, often necessitating a second surgery to enhance speech quality [2-9]. A common speech issue linked to corrected cleft palate is hypernasality, characterized by excessive nasal resonance during speech [1].

Diagnosing hypernasality requires evaluation by a speech-language pathologist, the result of which leads to interventions such as additional surgery or speech therapy [10]. However, accessing this diagnosis is often challenging for patients in low-income settings or regions with limited healthcare resources [11]. As a result, this project seeks to leverage OpenAI's Whisper audio model to detect hypernasality in audio samples of these patients, which can facilitate their access to speech pathology services. The results have shown that the proposed model has a test accuracy of 97% and an F1 score of 0.97 on public online patient data. Future work includes testing the model on protected patient data from Vanderbilt University Medical Center (VUMC) and extending its application to non-English languages.

## Goal
The objective is to develop an Whisper-based audio model to identify speech impairments.

## Methodology 
The whole process includes data processing, setting up a Whisper encoder, and training a neural network for classification. The baseline which uses SVM and random forest to classify is also included.

### Data Processing
Since the Whisper model prefers the sampling rate to be 16kHz and files to be in wav format, each of the audio files was resampled into 16kHz and converted into wav format from mp3 format in the data processing process. The wav format has higher resolution then mp3 format.
### Whisper Encoder 
The whisper models include whisper-base, whisper-small and whisper-tiny[12]. Each encoder processed the audio training data and then passed the results into a neural network classifier. 
### Neural Network
The neural network includes a series of linear and Rectified Linear Unit (ReLU) Layers. AdamW was used for optimization and cross-entropy was used for loss. The validation set was evaluated before the test dataset was evaluated. 

## Dataset
The data are publicly available patient voices from web-based sources, such as the Center for Disease Control and Prevention website. It includes 184 mp3 files, all of which are patient voice samples. 88 of the samples belong to the control group, which are patients without hypernasality and 96 of them are from the case group, which are patients with hypernasality. The audio samples have a duration from 0.44 to 9.35 seconds. 
The samples were separated into train and test sets. In total, the training set included 5.65 minutes of audio samples. 

## Results

### Traditional Transformer
The traditional transformer architecture consists of an encoder and decoder structure, each comprising multiple layers that perform complex functions. At the heart of the encoder and decoder lies the multi-head attention mechanism, which allows the model to focus on different parts of the input sequence when predicting each part of the output sequence. Each layer follows a specific order: multi-head attention is followed by a normalization step and a feed-forward network, with another normalization step thereafter. This architecture is powerful but computationally intensive, especially for long sequences, due to the quadratic complexity of the self-attention mechanism.

### Fourier Transformer
The Fourier transformer is an innovative adaptation of the traditional transformer that replaces the multi-head attention mechanism with a Fourier transform-based approach. The key distinction lies in how the attention is computed:
- Instead of calculating the attention weights for each element in the sequence with every other element (which is computationally expensive), the Fourier transform efficiently captures frequency-based relationships in the sequence.
- This not only reduces the computational complexity but also allows the model to handle much longer sequences without a significant increase in computation time.
- The feed-forward and normalization layers after the Fourier transform in the encoder remain unchanged, preserving the structure that enables complex representations.

### LSTM
Long Short-Term Memory (LSTM) networks are a special kind of Recurrent Neural Network (RNN) capable of learning long-term dependencies. LSTMs are explicitly designed to avoid the long-term dependency problem, remembering information for extended periods through their internal gates:
- **Forget Gate:** Decides what information should be thrown away or kept.
- **Input Gate:** Updates the cell state with new information.
- **Cell State:** Carries relevant information throughout the processing of the sequence.
- **Output Gate:** Decides what the next hidden state should be.

### Comparison
- **Computational Efficiency:** The Fourier transformer is computationally more efficient than the traditional transformer. By converting the attention mechanism into the frequency domain, it sidesteps the need for quadratic computation with respect to sequence length.
- **Handling Long Sequences:** The Fourier transform's ability to work in the frequency domain means it can handle long-range dependencies and large sequences better than the traditional multi-head attention mechanism.
- **Preserved Depth and Complexity:** Despite the simplification in attention calculation, the depth and complexity of the model remain intact. The feed-forward and normalization layers ensure that the model can still learn complex representations and perform sophisticated transformations on the input data.

## Code Demonstration
[Fourier Transformer](https://github.com/Zoliverling/Linear_Transformation/blob/main/Fourier_Transformer.py)

[Code Demonstration](https://github.com/Zoliverling/Linear_Transformation/blob/main/Linear%20Transformer.ipynb)

## Interactive Demonstration
[Demo with Gradio](https://github.com/Zoliverling/Linear_Transformation/blob/main/stock_prediction.ipynb)


## Critical Analysis and Future Directions
### 1. Data Exploration Enhancement
To further enhance the model's training efficacy, an in-depth exploration of additional datasets is recommended. This will not only enrich the model's learning base but also potentially uncover new patterns and insights that could improve predictive accuracy.

### 2. Expansion to Diverse Application Fields
Investigating the model's adaptability across various domains is advised. By exploring possible implementations in other fields, such as healthcare, energy, or retail, the utility and versatility of the model could be substantially increased, thereby broadening its application spectrum.

### 3. Performance Optimization through Integration
Consideration should be given to augmenting the current model by integrating it with other models or methods. This hybrid approach could leverage the strengths of multiple modeling techniques, potentially leading to significant improvements in performance metrics such as accuracy, speed, and robustness.


## Useful Resources
[A Financial Time-Series Prediction Model Based on MultiplexAttention and Linear Transformer Structure](https://www.mdpi.com/2076-3417/13/8/5175)

[Forecasting Economics and Financial Time Series: ARIMA vs. LSTM](https://arxiv.org/abs/1803.06386)

[Explore Attention Machanism](https://www.3blue1brown.com/lessons/attention)

[Time Series Forecasting](https://www.tableau.com/learn/articles/time-series-forecasting)

## References
1. Peter A. Mossey and Eduardo Castilla. Global Registry and Database on Craniofacial Anomalies: Report of a WHO Registry Meeting on Craniofacial Anomalies. World Health Organization, 2003.
2. M. A. Hardin-Jones and D. L. Jones. "Speech production of preschoolers with cleft palate." The Cleft Palate-Craniofacial Journal, vol. 42, no. 1, pp. 7–13, 2005.
3. M. Iglesias, D. P. Kuehn, and H. L. Morris. "Simultaneous assessment of pharyngeal wall and velar displacement for selected speech sounds." J Speech Hear Res, vol. 23, no. 2, pp. 429–46, 1980.
4. S. N. Chiang, et al. "Buccal Myomucosal Flap Repair for Velopharyngeal Dysfunction." Plast Reconstr Surg, 2023.
5. A. Ysunza, et al. "Velopharyngeal surgery: a prospective randomized study of pharyngeal flaps and sphincter pharyngoplasties." Plast Reconstr Surg, vol. 110, no. 6, pp. 1401–7, 2002.
6. S. Naran, M. Ford, and J. E. Losee. "What's New in Cleft Palate and Velopharyngeal Dysfunction Management?" Plast Reconstr Surg, vol. 139, no. 6, pp. 1343e–1355e, 2017.
7. O. Jackson, et al. "The Children's Hospital of Philadelphia modification of the Furlow double-opposing Z-palatoplasty: 30-year experience and long-term speech outcomes." Plast Reconstr Surg, vol. 132, no. 3, pp. 613–622, 2013.
8. L. L. D’Antonio and N. J. Scherer. "Communication disorders associated with cleft palate," in Comprehensive Cleft Care, J. E. Losee and R. E. Kirschner, Eds. McGraw-Hill Medical, New York, 2009, pp. 569–588.
9. D. M. Fisher and B. C. Sommerlad. "Cleft lip, cleft palate, and velopharyngeal insufficiency." Plast Reconstr Surg, vol. 128, no. 4, pp. 342e–360e, 2011.
10.Parameters For Evaluation and Treatment of Patients With Cleft Lip/Palate or Other Craniofacial Differences. The Cleft Palate-Craniofacial Journal, vol. 55, no. 1, pp. 137–156, 2018. DOI: 10.1177/1055665617739564.
11.M. Baigorri, et al. "Barriers and Resources to Cleft Lip and Palate Speech Services Globally: A Descriptive Study." Journal of Craniofacial Surgery, vol. 32, no. 8, pp. 2802–2807, 2021. DOI: 10.1097/SCS.0000000000007988.
12.Transformers Documentation: Whisper. HuggingFace. Retrieved from https://huggingface.co/docs/transformers/model_doc/whisper
13.Prevention, C.f.D.C.a. CDC’s Developmental Milestones. 2023 June 6, 2023; Retrieved from: https://www.cdc.gov/ncbddd/actearly/milestones/index.html.
14. Unit, E.O.H. Let’s Talk: Tips for Building Your Child’s Speech and Language Skills. Retrieved from: https://www.youtube.com/watch?v=K0aHjxzDb7I.
15. ENT, F. What is VPI (Velopharyngeal Insufficiency)? ; Available from: https://www.youtube.com/watch?v=WM5fVCdBPHs.
16. Learning, J.B. Hypernasality. Retrieved from: https://www.youtube.com/watch?v=KWz5_fpnZYc
17. Khan, A., Farhan, M., & Ali, A. 2011. Speech Recognition: Increasing Efficiency of Support Vector Machines. International Journal of Computer Applications, 35(7). Retrieved from https://arxiv.org/ftp/arxiv/papers/1204/1204.4257.pdf.
18. LEADERSproject. Cleft palate speech therapy using books for phrases and sentences. Retrieved from: https://www.youtube.com/watch?v=1nHhqdCnwBI.
19. Chicago, S.C.s. New app may help kids with cleft palate speak easier. Retrieved from: https://www.youtube.com/watch?v=5fubZitvY-Q.
20. Therapy, B.P.a.H. Case Study: Pediatric Speech Therapy for Cleft Palate. Retrieved from: https://www.youtube.com/watch?v=noUGRjClUg4.
21.LEADERSproject. Cleft palate speech and feeding: addressing speech and language before surgery. Retrieved from: https://www.youtube.com/watch?v=-sEt3i0sHr4


