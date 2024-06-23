# U.S.-Patent-Phrase-to-Phrase-Matching
Kaggle competition 


### we have tow approaches :
- #### first : 
    -  fine-tuning with transformers
    -  useing best practice hyperpramters from other comptitions wthout tuning them (i like to spend more time with the second approach^^) 
    -  working more and more with the inputs and the data that will finetune with.
    -  testing coustom loss function, pearson correlation, worked well with the first eboach only!, need more debug but the same was worked with the pytorch model.
    -  working with the task as a regression task with only one label.
- ### Second approach:
    -  taking the last hidden state of the encoder based model ( like debarta ) then build some layears above it.
    -  training the whole model with hyperpramters that were tuning well
    -  using custom loss function with some tricks, pearson correlation (pearson correlation is the evaluate metric of the competition) worked better than BCEWithLogitsLoss
    -  using AWP from the third eboach
    -  using attention pooling and an initialization weights that worked better than the default
    -  shuffle a part of the input sequence (for every example) in every traing step (the trick worked well in the cross validation score)
    -  a trick for GPU limitations when working with bigger models and more long input sequnces :
        -  in every batch, declare different max_len for the tokenizar based on the max input length in this batch, after sorting the df, this trick make me able to use deberta-v3-large with a long sequnce inputs and batch size = 4 
    -  last save the best models from every training (the best score on the validation set for every fold), then averaging them.
- ### General tricks :
    -  i use StratifiedGroupKFold for cross validation for two reasons, first the data was groubed by the anchor (very important note), second the data is imbalance.
    -  adding related data from other sources
    -  groupby(['anchor', 'context'])['target'] added to the input 
- ### tricks I want to implement in the comming version :
    -  work more with the nn model, test add bi-lstm and test more tricks in this point.
    -  save the best models from the first approach and use in the final subbmission else.
    -  test different and bigger LLM anc combine the results of the best in more robust way than just averaging.
    -  freaze almost all deberta layers, and almost work only with the head added part.
