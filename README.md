# CryptoniteAnalysis
## Authors
This is a joined project of Hsin-Chun Yin, Tahila Mescheloff, Peleg Oppenheimer and Elchanan <last_name?>. 


## Description
In this project we are trying to solve a dataset called "cryptnite". A detail of the dataset can be found in this paper: 

	https://arxiv.org/pdf/2103.01242.pdf
And it's official github: 

	ProofOfConcept/ProcessedDatasets/
In short this dataset is a set of cryptic puzzles. Each clue contains a part that defines the answer and a part that suggests the word play. And we are trying to see how can we use tools in NLP to solve the puzzles.  


## Datasets
The original dataset can be download from the official github.  
Or from my google drive (Because my github cannot upload big files):

	https://drive.google.com/drive/folders/177zxNj-O8mpLa1-U9Aj0j2_uH2jcW1S5?usp=sharing
All the other data, for example, the preprocessed dataset for proof of concept is, as well as the trained model savetensors are also on my drive.  
1. Saved models for multiple choice (PoC): ProofOfConcept/Models  
2. Saved preprocesed dataset for multiple choice (PoC): ProofOfConcept/ProcessedDatasets

## Multiple Choice *2024.07.07*
My first step is trying to see if the language model can capture some information of the answer from the clue sentences. So we decided that we will let the model do multiple choice. We generated three answers in addtion to the correct answer. The geenrated answers have the same structure of the original answer (same number of words and same number of letters for each word).  
Before training, the model's accuracy is roughly 22%, after training, the model's accuracy went up to 88.8%, indicating that the model does capture some informations from the clue sentences.  
Potential problem for this approach is that the answers are randomly generated, so even though they are actual english words, it shouldn't be hard for the model to capture some small corelation between the clue sentence and the answers. But this is just a proof of concept, I am happy with the result that there is some result : ). 
A potential extension for the multiple choice task is, we can run our model over all the words with the same structures: for example, there are about a few thousands of words that's with length 5, so we will run the model over all these words, and choose the most probable answer. But the required computational power would be huge, so far I am not continuing with this thread.   

## Generation - Benchmark 
The second step is trying trying to set a benchmark for the actual task: How well would the model perform after fine tuning? Give that the input is just the clue sentence, and the output is just the word.  
For a little improvement than just train the model on the raw output, I would apply some tricks so that the model will output at least words with the given structure (number of words and number of letters per word). One trick I can think of for now, is to add the structure information to the output (and label) before calculating the loss. So that the model will try to align the structure information part.(*2024.07.07*: waiting for more opinions)
 
