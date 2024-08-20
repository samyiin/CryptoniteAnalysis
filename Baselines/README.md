# Notes
Throughout the iterative development process, I added more training techniques:
In the beginning, I just used input and output, the classic DL cycle. It’s in the Raw.ipynb notebook. 
Then I discovered something called teacher forcing, basically the idea is to provide a real label to the generative model during training. Go search it on google. It’s in the TeacherForcing.ipynb notebook.
Then I need to tune bigger models, so I use Low Rank Adaptation (LoRA), which is in the LoRATeacher.ipynb notebook, because I use LoRA on top of teacher forcing. 
But there are still some places for improvement with my training process (especially after I discovered the adapter framework, I recorded these potential improvements in the Training_LoRA.ipynb notebook. But for now we are moving towards another direction, so we will freeze this thread for now. 

The reason I didn’t use the trainer class of hugging face is because I want to leave space for customizing the training process (RL-PPO, adding and saving adapters), but I am not sure if this is a correct decision so far since so far, all the customizations can be achieved through overriding some functions of the trainer class. 

# Seq2Seq Summarization
The result for the baselines are:  
1. 'facebook/bart-base' (batch_size=16, epoch=5, lr=5e-05):   
'avg_loss': 0.02670454930007223, 'accuracy': 0.013724815536949956


2. 'facebook/bart-large-cnn' (batch_size=16, epoch=3, lr=5e-04, LoRA):   
'avg_loss': 0.9476232418943064, 'accuracy': 0.0


3. 'google-t5/t5-small' (batch_size=16, epoch=5, lr=5e-05):   
'avg_loss': 0.01681845185953292, 'accuracy': 0.0056581412241465


4. 'google-t5/t5-large' (batch_size=16, epoch=3, lr=5e-04, LoRA, sub-sampled):  
'avg_loss': 0.014473656484027674, 'accuracy': 0.012080896127231715

There are a few findings: 
Every one performs horribily. Somehow bart base preforms better than large models. 
If given a smaller dataset to train on, then the accuracy is really high. This may lead to a conclusion that maybe we should classify our dataset first, and LoRA fine tune for each subtask. 
Maybe we need to train on more epochs?
 
But why does bart-large perform so bad?  
For bart-large and t5-large, I used the same training script, and the loss is reduced during training, so it rules out the possibility of bugs.




