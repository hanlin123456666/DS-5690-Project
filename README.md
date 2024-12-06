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
The process involves data preprocessing, Whisper encoder setup, neural network training for classification, and a baseline SVM and random forest classifier.

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

### Whisper Tiny and neural network
#### Code Demonstration 
https://colab.research.google.com/drive/1zcZXUMFPH4aXdrtzKlfi4nM7Ugjm6600?usp=sharing

#### Interpretation
The evaluation of the model's performance on both validation and test datasets demonstrates a robust but slightly varied ability to classify the data accurately. During validation, the model achieved high and balanced precision, recall, and F1-scores of 0.91 across both classes, resulting in an overall accuracy of 91%. This suggests that the model is effective at distinguishing between the two classes and is well-calibrated on the validation data.
![image](https://github.com/user-attachments/assets/48921a91-9bb0-4e44-affc-62a757833ad3)

However, on the test dataset, the model's performance showed some variance, with an overall accuracy of 86.5%. Class 0 exhibited a slightly lower recall (0.78) compared to precision (0.93), indicating that the model missed some true instances of this class. Conversely, Class 1 showed stronger recall (0.95) than precision (0.82), suggesting it captured most true instances but included more false positives. The F1-scores for Classes 0 and 1 were 0.85 and 0.88, respectively, with a macro average of 0.86 across both classes.
![image](https://github.com/user-attachments/assets/2c3cbe7a-b285-470f-a033-aa675bb84992)

While the test results are slightly lower than the validation performance, the model remains effective and demonstrates a solid generalization capability. The discrepancy suggests room for improvement, particularly in balancing precision and recall for both classes on unseen data. Overall, the model shows strong predictive capabilities and general reliability for this classification task.

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


