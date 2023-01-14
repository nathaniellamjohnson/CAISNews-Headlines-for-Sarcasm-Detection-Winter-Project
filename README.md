# Detecting Sarcasm in News Headlines - CAIS++ Winter Project 

### Submission by Nathaniel Lam Johnson, nlj@usc.edu

*Transfer Learning for News Headline Sarcasm Detection Using BERT Based Supervised Fine-Tuning*

The dataset used was "News Headlines Dataset for Sarcasm Detection"<sup>1</sup>, a collection of news headlines pulled from the Onion and the Huffington Post. The dataset contains a string of the article headline, the article hyperlink, and a value indicating whether the headline is sarcastic or not. Preprocessing steps involved importing and concatenating the 2 JSONs that had a mismatch in label orders. This decision was made because the data seemed to be evenly split between the 2 JSONS, so saving one for a final validation would probably be unreasonable.

I decided to perform transfer learning using a pre-trained BERT model and a final linear layer because the dataset was too small to train a full transformer model on. No extra data augmentation could occur because of the nature of the task, so thus a transfer learning model was chosen. 

The hyperparameters settled on are as follows
1. Preexisting Model - The "bert-based-uncased" model in BertForSequenceClassification was used because the previously trained asked language modeling and next sentence prediction tasks create an inner representation of English that was useful for the downstream sarcasm prediction task.
2. Optimizer - AdamW was chosen because it was used in the original BERT paper. AdamW was used over other optimizers because it generally performs better than most optimizers. 
2. Learning Rate - The learning rate chosen was "2e-5". This was mainly chosen because it fell within the recommended learning rate bounds. Retrospectively, this seemed to be an appropriate learning rate as it converged quickly.
3. Epochs - The number of epochs chosen was 4. This was chosen because it fell within the BERT paper's recommended epochs. In retrospect, the major increase in accuracy happened in epochs 1 and 2 with minor improvements in epoch 3 and 4. Future work could point to more efficient training epoch and rate.
4. Batch Size - The batch size chosen was the recommended 32 for transfer learning. 


The model achieved a final accuracy of 0.992955 on the validation set. Additionally, when tested on article headlines from a variety of news sources such as the NYT, AP News, Huffington Post, The Onion, and The Sack of Troy, the model correctly classified every headline. (Needs to check out recall & other stats, but model did not save correctly)

One shortcoming is the inability for the model to accurately classify a headline from Infowars. One reason might be data limitations. The model's understanding of sarcastic headlines is trained on generally the news headlines from the Onion, which might mimic and satirize far-right sources like Infowars. For example, in the last code cell, the first headline comes from Infowars, a right-biased news source, is incorrectly identified as sarcastic. However, the next headline comes from FOX news and is correctly identified as non-sarcastic. 

Defining whether Infowars and similar lesser news establishment is similar enough to the trained data to be fed in is most likely a future task. It also remains controversial especially because some people may consider Infowars/similar sources news or satire depending on political affiliation and beliefs. his needs to be looked in to more to ensure that the data collection sources, and methods have not restricted the model to mis predict sources unlike the training data. (By the way I don't support or watch Infowars, but it's an interesting conversation to have nonetheless.)

One of the concerns that many people have is that NLP remains unable to detect sarcasm, a form highly dependent on non-verbal cues, such as logical mismatches and body language. This project shows that at least on some superficial level, NLP models can understand sarcasm. In the future, it would be interesting to analyze comedy show transcripts to identify and potentially predict jokes. Additionally, extending this understanding of sarcasm might work for the additional modalities provided by video. 



Sources/Acknowledgements
---
1. Misra, Rishabh and Prahal Arora. "Sarcasm Detection using Hybrid Neural Network." arXiv preprint arXiv:1908.07414 (2019). Misra, Rishabh and Jigyasa Grover. "Sculpting Data for ML: The first act of Machine Learning." ISBN 9798585463570 (2021).
2. Devlin, Jacob, et al. "Bert: Pre-training of deep bidirectional transformers for language understanding." arXiv preprint arXiv:1810.04805 (2018).
