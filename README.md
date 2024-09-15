# Cryptonite Analysis
This project is aim to improve the LM's ability to solve cryptic crossword puzzle through various techniques. We tried many methods, including various fine turing techniques and prompt engineering techniques. We are getting our cryptic crossword puzzle through a dataset called "Cryptonite", the baseline for this dataset is 7.64% from fine tuning a T5-large model. here is the paper for it: 

    https://arxiv.org/abs/2103.01242

The Whole project is run on Google Colab. So this github is just a mirror to the google drive. All the fine-tuned models are stored on the google drive. For expensive LLM api function calls, there are function call caches on my google drive. So if you want to run the project, please go visit the google drive. However, if you want to run this project locally, it is not difficult either. Simply delete the "mount to google drive" steps in every notebook. And os.chdir to the root directory of this project (CryptoniteAnalysis), then everything should run smoothly. Here is my google drive's url:

    https://drive.google.com/drive/folders/177zxNj-O8mpLa1-U9Aj0j2_uH2jcW1S5?usp=sharing

# Fine tuning models
We tried several ways trying to improve the results of fine tuning: Teacher Forcing, LoRA, Transfer Learing (Naive). We fine tuned 5 models: bart-small, bart-large-cnn, T5-small, T5-large, Roberta-base. However, non of the result is ideal. 
Future direction: There is an article that tried curriculum learning, and improved the results to 20% on a different cryptic crossword dataset. Customizing tokenizer in order for the model to understand the wordplay is also another direction. 

## How to see the evaluation results?
The results of all 5 models are in the notebook Baselines/Evaluations.ipynb
## How to replicate our results? 
(Assume you are on google drive) 
### Seq2Seq models
1. Run the Baselines/Seq2Seq/Preprocessing.ipynb notebook
2. Run the rest of the notebooks under Baselines/Seq2Seq
After training (around 15 hours for one model, 5 epochs, on GPU), the results and fine-tuned models will be under Baselines/Seq2Seq/TrainingData/ directory.
### Bert-like models
1. Run the Baselines/MultipleChoices/Preprocessing.ipynb notebook
2. Run the Baselines/MultipleChoices/MultipleChoice.ipynb notebook
3. Run the Baselines/MultiChoiceToGen/Multi_Choice_to_Generative.ipynb notebook
After training (around 10 hours in total), the results and fine-tuned models will be under Baselines/MultiChoiceToGen/Results/ directory.

# Prompt Engineering
We slowly came to the realization that, maybe it's not the probelm of fine tuning techniqes; Maybe the problem for not being able to improve the results is because our models are not big enough. So we start trying prompt engineering techniques. We started with zero-shot querying ChatGPT. The results are really good. So we start to incorprete prompt engineering techniques, such as CoT, interative prompting and in context learning. Evetually we found that one of the suitable methods to tackle this problem, is automatic strategy selection combined with inference scaling techniques. In plain English, it's basically telling the model to make a CoT plan, and follow the reasoning steps of the CoT. After the model output the response, tell the model to refelct on it's response again and again and iteratively refining the response. From online source we gathered, this is also similar to how gpt-o1 is trained. (At the time we finish this project, gpt-o1 is just published).
## How to see the evaluation results?
The results are at the botom of each notebook, just scroll to the bottom to see them. (By the time this github is written, gpt-o1 only opens to website users. So we recorded the results in the docx file. )
1. Zero shot QA: Located at PromptEngineering/NaiveQA.ipynb

    1.1. Zero shot QA for gpt-o1 and gpt-o1-mini:  PromptEngineering/Openai\ o1\ response.docx
2. Handcrafted CoT: Located at PromptEngineering/CoT.ipynb
3. Auto Strategy Selection: Located at PromptEngineering/AutoStrategySelection.ipynb
   
    3.1. Auto Strategy Selection + Reflection is located in the same file. 
## How to replicate our results? 
1. Run the PromptEngineering/Preprocessing.ipynb notebook
2. Run the rest of the notebooks under PromptEngineering/Seq2Seq
   
Takes about 3 hours for each experiment in Auto Strategy Selection

# Other resources in this github repo
## Handcrafted 5 step Auto Strategy Selection Context
In order to teach the LLMs to perform auto strategy selection, I designed a 5-step CoT to help LLMs select strategies. I spend a while to take 53 special examples from the book *How to Master the Times Crossword: The Times Cryptic Crossword Demystified [Moorey (2008)]*, and then carefullt explain the association process, not just "What is correct" but rather "How do you think of that?". It turns out to be a bit waste of time, because appearantly someone in OpenAI already tired to do so, and gpt-models will automatically perform strategy selection when they encounter Cryptic crossword puzzles. The one thing that is trained during this process is probably me myself, I became much better at cryptic crossword puzzles after this. But if anyone is interested in becoming a crossword master, the 53 hand crafted examples are in PromptEngineering/InContextLearningExamples/. 
## Wordplay Classifcations
Also, after these days of research, I developed a new classification for wordplay catagories, the new classification might be better for machine to understand how to select strategies when they are performing surface reading, what kind of wordplay to perform, performing on which part of the clue, etc. So if you are interested, it's under PromptEngineering/DifferentClassifications.docx

 # Results
 All the results is already organized in the CryptoniteAnalysis.pdf. In short, we improved the results from 7.64% to 29.5% (Tested on partial dataset due to budget). The current STOA we can found on internet (before gpt-o1) is around 20%. So it is some kind of progress. 
