# Prompt optimization
A pipeline is created to test the different prompts.

First, the text file with the prompt is loaded. Then, the prompt is run on the LLM for each title in the dataset, and the model's response is added to the output dataset. For reproducible results, the model’s temperature hyperparameter is set to 0 and the seed to 42.
The newly labeled dataset is then saved as an excel file and the time needed to produce the response is generated in tokens per second (token/s). 
The process is repeated for each attempted prompt.

Once the workflow pipeline for testing the prompts has been defined, the next step is to perform prompt engineering. All the attempted prompts can be found in the "prompt_optimization" folder of the current repository. 

The prompts are incrementally improved until optimal results are achieved. The most significant improvements are observed under the following conditions.
1. The request for leniency is included in both the prompt and the [system](https://github.com/ollama/ollama/blob/main/docs/api.md#generate-a-completion) parameter (prompt 3.2).
2. The model is instructed to follow the directions in a specific sequence to establish priorities (prompt 10).
3. Two-shots prompting (prompt 12).
4. Step-by-step reasoning (prompt 15).
5. Two-shots prompting + step-by-step reasoning (prompt 17).

# Analytic strategy
To determine the effectiveness of the LLM in accurately filtering out irrelevant papers, the quality of the prompts must be evaluated to identify the prompt that elicits the best labeling choices from the LLM. The LLM must label conservatively, ensuring no relevant titles are excluded. To evaluate the prompts two metrics are employed: confusion matrices and recall. 

The [confusion matrix](https://scikit-learn.org/stable/auto_examples/model_selection/plot_confusion_matrix.html) displays the number of false positives and false negatives predicted by the model. If the number of false negatives in the confusion matrix is zero, the optimal prompt has been identified, allowing the transition to the testing phase. 

[Recall](https://scikit-learn.org/stable/auto_examples/model_selection/plot_precision_recall.html) is a valuable metric when the cost of false negatives is high, even if it results in an increased number of false positives. The desired outcome is a recall of 1.00, which indicates the absence of false negatives, ensuring that no relevant title is incorrectly labeled as irrelevant. 

When a recall of 1.00 is reached, with zero false negatives displayed in the confusion matrix, the optimal prompt has been identified.