Throughout the iterative development process, I added more training techniques:
In the beginning, I just used input and output, the classic DL cycle. It’s in the Raw.ipynb notebook. 
Then I discovered something called teacher forcing, basically the idea is to provide a real label to the generative model during training. Go search it on google. It’s in the TeacherForcing.ipynb notebook.
Then I need to tune bigger models, so I use Low Rank Adaptation (LoRA), which is in the LoRATeacher.ipynb notebook, because I use LoRA on top of teacher forcing. 

I tried to tune flan-t5-large, it is possible, but I must use the A100 GPU, which is super expensive. So I turn to use the t5-large, which is what the original baseline in the cryptonite paper.

