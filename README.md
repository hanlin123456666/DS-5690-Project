# DS-5690-Project
# Utilizing Whisper for Identifying Cleft-Related Hypernasality in Patient Voice Samples
Hanlin Chen

hanlin.chen@vanderbilt.edu

## Introduction
Cleft palate is a craniofacial disorder that affects roughly 1 in 700 live births worldwide and is typically addressed through surgical intervention [1]. However, even after corrective surgery, approximately 30% of patients continue to experience speech deficiencies, often necessitating a second surgery to enhance speech quality [2-9]. A common speech issue linked to corrected cleft palate is hypernasality, characterized by excessive nasal resonance during speech [1].

Diagnosing hypernasality requires evaluation by a speech-language pathologist, the result of which leads to interventions such as additional surgery or speech therapy [10]. However, accessing this diagnosis is often challenging for patients in low-income settings or regions with limited healthcare resources [11]. As a result, this project seeks to leverage OpenAI's Whisper audio model to detect hypernasality in audio samples of these patients, which can facilitate their access to speech pathology services. The results have shown that the proposed model has a test accuracy of 97% and an F1 score of 0.97 on public online patient data. Future work includes testing the model on protected patient data from Vanderbilt University Medical Center (VUMC) and extending its application to non-English languages.

## Goal
The objective is to develop an Whisper-based audio model to detect hypernasality in audio samples of these patients.

## Methodology 
The process involves data preprocessing, Whisper encoder setup, neural network training for classification, and a baseline SVM and random forest classifier.

### Data Processing
Since the Whisper model prefers the sampling rate to be 16kHz and files to be in wav format, each of the audio files was resampled into 16kHz and converted into wav format from mp3 format in the data processing process. The wav format has higher resolution then mp3 format.
### Whisper Encoder 
The whisper models used include whisper-base, whisper-small, and whisper-tiny[12]. Each encoder processed the audio training data and then passed the results into a neural network classifier. 
### Neural Network
The neural network includes a series of linear and Rectified Linear Unit (ReLU) Layers. AdamW was used for optimization and cross-entropy was used for loss. The validation set was evaluated before the test dataset was evaluated. 

## Dataset
The data are publicly available patient voices from web-based sources, such as the Center for Disease Control and Prevention website. It includes 184 mp3 files, all of which are patient voice samples. 88 of the samples belong to the control group, which are patients without hypernasality and 96 of them are from the case group, which are patients with hypernasality. The audio samples have a duration from 0.44 to 9.35 seconds. 
The samples were separated into train and test sets. In total, the training set included 5.65 minutes of audio samples. 

## Results
### SVM
#### Code Demonstration 
https://drive.google.com/file/d/1XvwIqfNyRLnLQzj3rf818f2ViB6QH0Nn/view?usp=sharing

#### Interpretation
The SVM model achieves an accuracy of 85% on validation data. Class 0 has a high precision of 93%, indicating that most of the predicted instances of this class are correct. However, its recall is slightly lower at 74%, meaning the model misses some true cases of class 0. This results in an F1-score of 0.82 for class 0, reflecting a good but not perfect balance between precision and recall. On the other hand, class 1 achieves a precision of 79% and a notably high recall of 95%, indicating the model captures most of the true instances of this class. The F1-score for class 1 is 0.86, slightly outperforming class 0. The macro average F1-score of 0.84 and the weighted average F1-score of 0.84 suggest balanced and reliable performance across both classes. Overall, the validation results indicate that the model performs well in distinguishing the two classes.
![image](https://github.com/user-attachments/assets/2e493031-8911-4c66-b1fd-06d923f779b8)

However, the SVM model’s performance on the test dataset reveals significant challenges, with an accuracy of 58.93%. While class 0 achieves perfect recall (1.00), its precision is only 59%, suggesting a high false positive rate, which inflates its F1-score to 0.74. In contrast, the model fails entirely for class 1, with both precision and recall at 0.00, resulting in an F1-score of 0.00. This indicates that the model predicts no instances of class 1 correctly, heavily favoring class 0 instead. The macro average F1-score drops to 0.37, and the weighted average F1-score is 0.44, emphasizing the imbalance in the model's predictions. The drastic drop in performance from validation to test suggests overfitting to the validation data or an inability to generalize well to the test data. 
![image](https://github.com/user-attachments/assets/8548740b-5610-4ac0-afd5-cfcf49da0988)

### Random Forest
#### Code Demonstration 
https://drive.google.com/file/d/1XvwIqfNyRLnLQzj3rf818f2ViB6QH0Nn/view?usp=sharing

#### Interpretation
The model achieves an accuracy of 94.87%. Class 0 achieves perfect precision (1.00), meaning all predicted instances of class 0 are correct. Its recall, however, is slightly lower at 89%, indicating that some true instances of class 0 are missed. This results in an F1-score of 0.94, showcasing strong but slightly imbalanced performance. Conversely, class 1 achieves a precision of 91% and perfect recall (1.00), indicating the model captures all true instances of this class but has a few false positives. The F1-score for class 1 is 0.95, slightly outperforming class 0. The macro and weighted averages for precision, recall, and F1-score are all 0.95, reflecting balanced and robust performance across both classes.
![image](https://github.com/user-attachments/assets/b204d255-75f6-4b87-b032-5d52fc6ff22a)

The model's performance on the test dataset drops significantly, with an accuracy of 60.71%, suggesting challenges in generalizing to unseen data. For class 0, the precision is 61%, and the recall is 94%, resulting in an F1-score of 0.74. This indicates that the model is effective at identifying the most true instances of class 0 but at the expense of a high false positive rate. In contrast, the model struggles significantly with class 1, achieving only 60% precision and a low recall of 13%, leading to an F1 score of 0.21. This suggests that the model fails to capture the most true instances of class 1 and is heavily biased toward class 0. The macro average F1-score drops to 0.48, and the weighted average F1-score is 0.52, highlighting the imbalance in predictions and the poor generalization to the test dataset, similar to SVM.
![image](https://github.com/user-attachments/assets/2722e826-3252-4ee1-9359-b88ee1293d5c)

### Whisper base and neural network
#### Code Demonstration 
https://drive.google.com/file/d/1XvwIqfNyRLnLQzj3rf818f2ViB6QH0Nn/view?usp=sharing

#### Interpretation
The model demonstrates strong performance on the test dataset with an accuracy of 84.44%. Class 0 achieves a perfect precision of 1.00, indicating that all instances predicted as class 0 are correct. However, the recall for class 0 is 68%, meaning the model misses a significant portion of true class 0 instances. This imbalance results in an F1-score of 0.81 for class 0. On the other hand, class 1 achieves a precision of 0.77 and a recall of 1.00, showing that the model captures all true instances of class 1, albeit with some false positives. The F1-score for class 1 is 0.87, reflecting better overall performance than class 0. The macro averages for precision, recall, and F1-scores are 0.88, 0.84, and 0.84, respectively, indicating a slight favoring of class 1. Overall, the test results highlight the model's ability to generalize well while showing some room for improvement in class 0's recall.
![image](https://github.com/user-attachments/assets/8db566b3-5ec2-48a2-894e-cca16eaee4b8)

The validation dataset reveals weaker performance, with an accuracy of 70.27%. Class 0 achieves a precision of 0.89, suggesting that most predicted class 0 instances are correct, but the recall is much lower at 44%, indicating that the model misses a majority of true class 0 instances. This leads to an F1-score of 0.59, reflecting an imbalance in the model's ability to identify class 0 accurately. For class 1, the precision is 0.64, with a high recall of 95%, indicating the model correctly identifies most true class 1 instances but includes more false positives. The F1-score for class 1 is 0.77, significantly outperforming class 0. The macro and weighted averages for precision, recall, and F1-scores are lower than those for the test dataset, suggesting that the model's performance on the validation data is less consistent, particularly for class 0. These results indicate the need for further tuning to improve recall for class 0 while maintaining the strong recall for class 1.
![image](https://github.com/user-attachments/assets/1f398921-c503-4854-a4c8-106dae379c8d)

### Whisper Small and neural network
#### Code Demonstration 
[https://colab.research.google.com/drive/1zcZXUMFPH4aXdrtzKlfi4nM7Ugjm6600?usp=sharing](https://colab.research.google.com/drive/1lHa0c4XX4eGBGlqXJg2YtNsxZpCHyzs2?usp=sharing)

#### Interpretation
The model demonstrates excellent performance on the validation dataset, achieving an accuracy of 93.33%. Class 0 has a precision of 95%, indicating that nearly all predictions for this class are correct, while its recall of 91% shows the model successfully identifies most true instances of class 0. This balance results in an F1-score of 0.93. Class 1 performs similarly well, with a precision of 92% and a recall of 96%, reflecting the model’s ability to correctly classify most true instances of class 1 while maintaining high precision. The macro and weighted averages for precision, recall, and F1-scores are all 0.93, indicating consistent and balanced performance across both classes. These results suggest the model is well-calibrated and highly effective on the validation dataset, with minimal room for improvement.
![image](https://github.com/user-attachments/assets/fd8d9e33-9062-483d-9bf3-2660030dc079)

On the test dataset, the model continues to perform strongly, achieving an accuracy of 91.89%. Class 0 shows a precision of 89% and a recall of 94%, resulting in an F1-score of 0.92, demonstrating the model’s strong ability to classify most instances of this class correctly. Similarly, class 1 achieves a precision of 94% and a recall of 89%, with an F1-score of 0.92, reflecting balanced and robust performance. The macro and weighted averages for all metrics are 0.92, confirming that the model generalizes well to unseen data. Although there is a slight drop in performance from validation to test, the model still performs reliably, demonstrating strong predictive capabilities and consistent classification performance across both datasets. This model has been selected for implementation in a user interface.
![image](https://github.com/user-attachments/assets/842c04e1-51be-461d-8c31-dfcc29ebd4e4)

### Whisper Tiny and neural network
#### Code Demonstration 
https://colab.research.google.com/drive/1zcZXUMFPH4aXdrtzKlfi4nM7Ugjm6600?usp=sharing

#### Interpretation
The evaluation of the model's performance on both validation and test datasets demonstrates a robust but slightly varied ability to classify the data accurately. During validation, the model achieved high and balanced precision, recall, and F1-scores of 0.91 across both classes, resulting in an overall accuracy of 91%. This suggests that the model is effective at distinguishing the two classes and is well-calibrated on the validation data.
![image](https://github.com/user-attachments/assets/48921a91-9bb0-4e44-affc-62a757833ad3)

However, on the test dataset, the model's performance showed some variance, with an overall accuracy of 86.5%. Class 0 exhibited a slightly lower recall (0.78) compared to precision (0.93), indicating that the model missed some true instances of this class. Conversely, Class 1 showed stronger recall (0.95) than precision (0.82), suggesting it captured most true instances but included more false positives. The F1-scores for Classes 0 and 1 were 0.85 and 0.88, respectively, with a macro average of 0.86 across both classes.
![image](https://github.com/user-attachments/assets/2c3cbe7a-b285-470f-a033-aa675bb84992)

While the test results are slightly lower than the validation performance, the model remains effective and demonstrates a solid generalization capability.

## User-interface (Whisper small model)
You can upload the audio file.
![image](https://github.com/user-attachments/assets/1a18f08c-8930-475b-9616-42bcceba7d0f)

You click submit and get the result.
![image](https://github.com/user-attachments/assets/1eb3618d-5d59-41de-a93a-990c02eab103)

## Resource links
Research Papers and Articles:
- https://arxiv.org/ftp/arxiv/papers/1204/1204.4257.pdf
Video resources
- https://www.youtube.com/watch?v=1nHhqdCnwBI
Repo
- https://github.com/speechbrain/speechbrain
- https://github.com/espnet/espnet
Organizations and initiatives
- https://www.nidcd.nih.gov/
- https://www.asha.org/
  
## Impacts, insights, and next steps
- Impact: This project has the potential to transform access to speech pathology services by providing an automated tool to detect hypernasality. It bridges the gap for patients in low-resource settings where access to speech-language pathologists is limited.

- Reveals or Suggests: The project reveals that deep learning models like Whisper can adapt well to specific speech pathology tasks with minimal customization. However, the discrepancies between validation and test performances in certain models suggest that further data diversity is crucial for improving generalization.

- Next Steps:
1. Expand Dataset: Incorporate real-world clinical data, including data from Vanderbilt University Medical Center (VUMC), to enhance generalizability.
2. Non-English Languages: Adapt the model to support multiple languages to cater to diverse populations.

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


