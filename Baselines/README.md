Throughout the iterative development process, I added more training techniques:
In the beginning, I just used input and output, the classic DL cycle. It’s in the Raw.ipynb notebook. 
Then I discovered something called teacher forcing, basically the idea is to provide a real label to the generative model during training. Go search it on google. It’s in the TeacherForcing.ipynb notebook.
Then I need to tune bigger models, so I use Low Rank Adaptation (LoRA), which is in the LoRATeacher.ipynb notebook, because I use LoRA on top of teacher forcing. 

I tried to tune flan-t5-large, it is possible, but I must use the A100 GPU, which is super expensive. So I turn to use the t5-large, which is the original baseline in the cryptonite paper.

# 2024.07.19
I finished training bart-base/large-cnn, t5-small/large for baseline, and there are two findings: T5 generally performs better than bart, and if given a smaller dataset to train on, then the accuracy is really high. This may lead to a conclusion that maybe we should classify our dataset first, and LoRA fine tune for each subtask. 
Maybe we can use option discovery to find subtasks?! A.K.A. types of problems that can be grouped together. I feel like this is a very novel approach, combining chain of though with options discovery in RL. 
Anyways, the result for the baselines are (* Notice that the accuracy includes the begin and end token):
1. bart-base: (batch_size=16, epoch=5, lr=5e-05) test_accuracy: 0.41233121225723324 train_accuracy: 0.7777777777777778
2. bart-large-cnn: (batch_size=16, epoch=3, lr=5e-04, LoRA) test_accuracy: 0.024999999999999974 train_accuracy：0.2631578947368421
3. t5-small: (batch_size=16, epoch=5, lr=5e-05) test_accuracy: 0.4931245750809229 train_accuracy: 0.5882352941176471
4. t5-large: (batch_size=16, epoch=3, lr=5e-04, LoRA, sub-sampled) test_accuracy: 0.5492572409585335 train_accuracy: 0.4918032786885246


For bart-large and t5-large, I used the same training script, and the loss is reducing during training, so it rules out the possibility of bugs. But why does bart-large performs so bad?
For results in each epoch I stored them on the drive. 
So what is the next step?

## Control Letter Length
[inprogress: *2024.07.24* , Hsin-Chun Yin]   
After we receive the baseline results, the first attempt to improve the model will be on the enumeration field. This decision is natural because when we are playing crossword puzzles, when we think of an answer, the first thing we check is if the word have the right length. So we will control the letter length and the word length for the output and see if it improves the result.   
**Calibrate Loss Function**
The first attempt will be to calibrate loss function: give huge punishment to words with incorrect length. This can be achieved by batch_decode the output of model in each iteration. 

**Pick Top Correct Words**  
The second attempt will be using the trained models from baseline, but only pick from the words with correct length. This can be done by setting mask for set of correct words. The problem for this approach is when answer looks like (1, 4, 3, 7), then the highest probability for each position after masking might not be the highest probability for the combination. 

**Customize Tokenizer**  
Make words with same length more similar to each other in addtion to "context". Such as adding an "regularize term".

## Chain of Thought
Another idea we have is to first identify the "definiton" and "wordplay" part of the clue, and then use models to find answers. 

## Characterize Datasets
Characterize different questions, and train LoRA on each. (Like what apple did)

## Combine Chain of thought/Characterize Dataset using the option discovery techniques
???

## In Context Learning
????


