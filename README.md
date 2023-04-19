# CITS4012 Assignment Specification - Wiki QA

**[XXX]** = Lecture/Lab Reference <br/>
**(Justify your decision)** = You are required to justify your final decision/selection with the empirical evidence by testing the techniques in the section 3. Model Testing. <br/>
**(Explain the performance)** = You are required to explain the trend of performance, and the reason (or your opinion) why the trends show as they do.

## Submission Due: 20 May 2023

This assignment can be done individually or in teams of two or three(max). We strongly encourage healthy collaboration. See the [University of Western Australia Working in Groups Guide](https://www.uwa.edu.au/students/-/media/Project/UWA/UWA/Students/Docs/STUDYSmarter/SS4-Working-in-Groups.pdf). If your team member does not engage or collaborate, please contact the unit coordinator ([Dr. Caren Han](mailto:caren.han@uwa.edu.au?subject=[CITS4012]%20Assignment%20Group)) with describing the contributions of each collaborator. We strongly recommend to start working early so that you will have ample time to discover stumbling blocks and ask questions.

In this assignment, you are to propose and implement a Wikipedia QA (Question Answering) framework using the Sequence model and different NLP features. The QA framework should have the ability to read documents/texts and answer questions about them. The detailed information for the implementation and submission step are specified in the following sections. Note that lecture note and lab exercises would be a good starting point and baseline for the assignment.

Note that we specified which lectures and labs are highly related!

**Table of Contents**
- [0. Important Dates (Very Important!!)](https://github.com/adlnlp/CITS4012_2023#0important-dates-and-schedule)
- [1. DataSet](https://github.com/adlnlp/CITS4012_2023#-1-dataset-4-marks)
- [2. QA Model Implementation](https://github.com/adlnlp/CITS4012_2023#2qa-model-implementation-10-marks)
- [3. Model Testing](https://github.com/adlnlp/CITS4012_2023#3model-testing-10-marks)
- [4. Documentation](https://github.com/adlnlp/CITS4012_2023#4documentation-6-marks)
- [Assignment Submission Method](https://github.com/adlnlp/CITS4012_2023#assignment-submission-method)
- [FAQ](https://github.com/adlnlp/CITS4012_2023#faq)

<br/>
<br/>


## <img src="https://em-content.zobj.net/thumbs/120/microsoft/319/calendar_1f4c5.png" width="30" />0.Important Dates and Schedule
The Important date for the Assignment can be summarised as follows:
- **Assignment Specification Release**: 20 April 2023
- **Assignment Group EOI Due**: 23 April 2023
- **Assignment Group Release**: 24 April 2023
- **Assignment Group Revision Due**: 26 April 2023
- **Assignment Submission Due**: 20 May 2023

All deadlines are **11:59 PM (AWST)**.

NOTE: 
**If you want to do individual or already have group members in mind**, Please Submit Group EOI by EOI Due (23 April 2023 11:59PM)
Otherwise, your group members will be selected by our teaching team. 

**After the Group Release date (24 April 2023 11:59 PM): YOU MUST CONTACT YOUR GROUP MEMBER BY THE GROUP REVISION DUE (26 April 2023 11:59 PM).**
**If you want to change your group to individual after EOI due,**  you can revise it by the Assignment Group Revision Due. However, in this case, you should get your team member's agreement. Please email the Unit Coordinator with your group member's agreement.

**If you do not respond your group member by the Group Revision Due (26 April 2023 11:59 PM)**, and your group member requests for working as an individual, you must work the Assignment individually.


<br/>

## <img src="https://em-content.zobj.net/thumbs/120/samsung/349/card-file-box_1f5c3-fe0f.png" width="30" /> 1. DataSet [4 marks]
| :exclamation:  You need to put the code that you conduct all actions for this section 1. Dataset to the [ipynb template](https://colab.research.google.com/drive/1flkFWGcD1S84HONhTGodsEudyPZjsJ75?usp=sharing) |
|-----------------------------------------|


In this assignment, you are asked to use Microsoft Research WikiQA Corpus. The WikiQA corpus includes a set of question and sentence pairs, which is collected and annotated for research on open-domain question answering. The question sources were derived from Bing query logs, and each question is linked to a Wikipedia page that potentially has the answer. More detail on this data can be found in the paper, [WikiQA: A Challenge Dataset for Open-Domain Question Answering](https://aclweb.org/anthology/D15-1237). 

- **Download datasets**:
You will be provided two datasets, including the [training dataset](https://drive.google.com/uc?id=1SXoGbD9WZHwhpqR-cBw7-8_7Ri06nIb6) and the [testing dataset](https://drive.google.com/uc?id=1TwuDSxlcAFDnTRpF-GRvqRXoR_UsJznH). Both datasets contain the following attributes: QuestionID, Question, DocumentID, DocumentTitle, SentenceID, Sentence, and Label (answer sentence, if label=1). If you want to explore or use the full dataset, you can download it via the [Link](https://www.microsoft.com/en-us/download/details.aspx?id=52419). However, the training and testing split should be followed by the one we provided.

- **Data Wrangling**:
You need to first wrangle the dataset. As you can see in the following Figure 1, each row is based on each sentence of the document. You need to construct three different types of data for training the model: Question, Document and Answer. To construct the document data, you should concatenate (with space or full-stop and space) each sentence that has the same DocumentID. For the answer data, use the sentence that has Label as 1.

Note: 1) Some questions may not have answers. (All labels are 0) 2) Some documents may have multiple questions.

<img src="https://github.com/adlnlp/CITS4012_2023/blob/main/Assi_figure1.PNG" width="80%"><p align="center">**Figure 1. WikiQA: Raw Data - Sample View**</p>


<br/>



## <img src="https://em-content.zobj.net/thumbs/120/whatsapp/326/desktop-computer_1f5a5-fe0f.png" width="30" />2.QA Model Implementation [10 marks]
| :exclamation:  You need to put the code that you conduct all actions for this section 1. Dataset to the [ipynb template](https://colab.research.google.com/drive/1flkFWGcD1S84HONhTGodsEudyPZjsJ75?usp=sharing) |
|-----------------------------------------|
You are to propose and implement the open-ended QA framework using word embedding, different types of feature combinations, and deep learning models. The following architecture describes an overview of QA framework architecture. (Click [This Link](https://github.com/adlnlp/CITS4012_2023/blob/main/assignment_overview.png) to view the high-resolution image.)

<img src="https://github.com/adlnlp/CITS4012_2023/blob/main/assignment_overview.png" width="80%"><p align="center">**Figure 2. Overview of the Architecture of the WikiQA**</p>

**1) Input Embedding [5 marks]**
<br/>
You are asked to generate the word vector by using the word embedding model and different types of features. For example, in Figure 2, the word ‘Perito’ is converted to the vector [word2vec, PER, 9, NNP, …].  

- **Word embedding** - refer to [Lab2](https://colab.research.google.com/drive/1KY63ItqLQshnM3CuF46Hxl4fq2_4k3en?usp=sharing), [Lab6](https://colab.research.google.com/drive/1aJB7LMVftUCDaN1OMIat6YyRvdTMgJf_?usp=sharing). 
You are to apply a pre-trained word embedding model, including word2vec CBOW or skip-gram, fastText, or gloVe. For example, you can find various types of pre-trained word embedding models from the following link:
[https://github.com/RaRe-Technologies/gensim-data#datasets](https://github.com/RaRe-Technologies/gensim-data#datasets)

- **Feature extraction**
Different types of features should be extracted in order to improve the performance and evaluate different combinations of model specification (will discuss more in the 4. Documentation section - Evaluation).  You are asked to extract at least **three types** of features from the following list:
1) *PoS Tags  -refer to [Lab6](https://colab.research.google.com/drive/1aJB7LMVftUCDaN1OMIat6YyRvdTMgJf_?usp=sharing)
2) *TF-IDF -refer to [Lab1](https://colab.research.google.com/drive/1w3vj8xzRzeHzZibCRxpNddgsbUCwf6T-?usp=sharing)
3) *Named Entity Tags -refer to [Lab6](https://colab.research.google.com/drive/1aJB7LMVftUCDaN1OMIat6YyRvdTMgJf_?usp=sharing)
4) *Word Match Feature: check whether the word appears in the question by using decapitalisation or lemmatization  -refer to Lecture 5 and [Lab5](https://colab.research.google.com/drive/1euiRIkQAaJS7Vjd5PRFxJHZw3aHn5d1K?usp=sharing)
5) *Others: Any one embedding type can be included if you have a logical reason why it requires to the wiki QA.



**2) Sequence QA Model (Recurrent Model with Attention) [5 marks]**
-refer to Lecture 7 and [Lab7(Will be released by 24/04/2023 9AM)]
In this QA framework, you are to implement a sequence model using RNN  (such as RNN, LSTM, Bi-LSTM, GRU, Bi-GRU, etc.) with attention. 

Figure 2 describes the Bi-LSTM-based model as an example.  As can be seen in Figure 2, your sequence model should have two input layers (one for the Question and another for Document) and one output layer (Answer). The output layer should predict the answer (start token and end token). Also, your sequence model is required to include an attention layer to get better performance (which needs to be presented in your architecture). The positions to add the Attention layer are recommended in the blue box (top-right) of Figure 2.

For the sequence model, you can use single or multiple layers but you should provide the optimal number of layers. 

The detailed architecture of your sequence model needs to be drawn and described in the report (refer to section 4 - Documentation). You need to justify (in the report) why you apply the specific type of RNN and put the attention layer in the specific position. 

The final trained model should be submitted in your Python package. 


<br/>


## <img src="https://em-content.zobj.net/source/skype/289/test-tube_1f9ea.png" width="30" />3.Model Testing [10 marks]
| :exclamation:  You need to put the code that you conduct all actions for this section 1. Dataset to the [ipynb template](https://colab.research.google.com/drive/1flkFWGcD1S84HONhTGodsEudyPZjsJ75?usp=sharing) |
|-----------------------------------------|

You are to implement a testing program with the trained model. When the testing program is executed, the program should show the testing result with different combinations of model specifications (different features that you used in section 2.1). The testing result should include precision, recall and F-value -refer to [Lab4(Exercise)].

You need to write a manual (readme) for the assessor. Your manual should guide how to test your program and also includes a list of packages (with version) that you used. If you work on Google Colab or Jupyter Notebook (.ipynb), your manual should guide the assessor on where to upload the required files (trained model, dataset, etc.). Note the assessor will use Google Colab to open your ipynb file. Unless you have a function that downloads required files from URL or Google Drive. 

The testing result and discussion should be described in the report  (refer to section 4 - Documentation).


<br/>


## <img src="https://em-content.zobj.net/thumbs/120/facebook/355/page-facing-up_1f4c4.png" width="30" />4.Documentation [6 marks]
You should submit pdf version of the assignment report (8 pages Maximum - excluding reference and appendix). The detailed documentation requirement for each section can be found above section 1. DataSet, 2. QA Model Implementation, and 3. Model Testing.

**NOTE:
The justification MUST be based on the previous literature reference (incl. international conference or journal publication) or empirical evaluation. (Check the definition  empiritcal evaluation 
We do not mark the section (or all assignment) if it is not implemented as described in the report.**



<br/>


## <img src="https://em-content.zobj.net/thumbs/120/whatsapp/326/envelope-with-arrow_1f4e9.png" width="30" />Assignment Submission Method
**Due Date:** 11:59 PM, Saturday 20 May 2023

**Submission:** LMS Assignment Submission Box (The submission box will be opened on 1 May 2023)

**You Must Submit Three Files:**
- pdf file: a report (documentation) with the given template
(file name: cits4012_your_groupid.pdf)
- ipynb file or python packages (.py files): An ipynb file or python package that includes all your implementation (the implementation is described in the following sections - dataset, NER model, and testing-leaderboard)
(file name: cits4012_your_groupid.ipynb)
- zip file: A zip file that contains trained models, dataset, readme - if necessary, and all other required files that you used for your program.
(file name: cits4012_your_groupid.zip)


## <img src="https://em-content.zobj.net/thumbs/120/google/350/person-raising-hand_1f64b.png" width="30" />FAQ
**Question: My Model performs really bad (Low F1). What did I do wrong?**<br/>
Answer: Please don't worry about the low performance as our training dataset is very small and your model is very basic deep learning model. Of course, there is something wrong with your code if it comes out below 10% (0.1).

**Question: How can we make POS Tag or NER Tag to the embedding?**<br/>
Answer: You can either use it as a categorial embedding (e.g. putting the POS tag index- like Noun-->0, Verb -->1, etc) or applied word embeding (e.g. Extracting the Pretrained Word2Vec embedding for the term 'Noun' or 'Verb'). If you have any other approach to apply, please proceed! (Note you need to have a justification - why you apply it by using theoretial justification or empirical evaluation) 

**Question: Do I need to save the word embedding model or Recurrent models?**<br/>
Answer: We highly recommend you to save your best word embedding model, and load it when you use it in your code. Otherwise, it is sometimes removed and overwrite all your code so that you need to re-run the whole code again.

**Question: Is there any other marking scheme/marking criteria available for the assignment?**<br/>
Answer: The assignment specification is extremely detailed. The marking will be directly conducted based on the specification. It means you will get the full mark if you fulfill all the requirement that we mentioned in the specification.

**Question: What do I need to write in the justification or explanation? How much do I need to articulate?**<br/>
Answer: We recommend conducting empirical evaluation. Empirical evaluation refers to the appraisal of a theory by observation in experiments. The key to good empirical evaluation is the proper design and execution of the experiments so that the particular factors to be tested can be easily separated from other confounding factors. Hence, visualizing the comparison of different testing results is a good way to justify your decision. Explain the trends based on your understanding. You can find another way (other than comparing different models) as well - like citing the peer-reviewed publications. 
