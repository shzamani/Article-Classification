### Problem Description
We want to create a service that automatically classifies articles into related groups by detecting patterns inside the dataset.

> Credit: This ML challenge is designed by iSentia. For more details, please check their repository available at [here](https://bitbucket.org/isentia/coding-challenge-ml/src/master/).

### Solutions
I have built the following solutions for this challenge: <br>
LDA+CNN: [[Notebook]](https://github.com/shzamani/Article-Classification/blob/master/cnn_article_classifier.ipynb) [[Colab]](https://colab.research.google.com/drive/1oL-_w1vj9EQy5EjQ7PaygRcbVRu-OwPY?usp=sharing)<br>
LDA+BERT: [[Notebook]](https://github.com/shzamani/Article-Classification/blob/master/bert_article_classifier.ipynb) [[Colab]](https://colab.research.google.com/drive/1YDbJy9uvPFbAnitj7i_CCuFINXg-FPBI?usp=sharing)

### Extra Questions
* How do you evaluate the accuracy and correctness of your model(s)?
  * Since I turned this challenge into a supervised-classification problem by taking the output of LDA (topics) as **true labels** (ground truth), I used _accuracy_ metric to evaluate the performance of these models. Other metrics such as _Precision (P)_ and _Recall(R)_ could also be used to show _"to what extent these models are good"_. Another metric that I could use is [classification report](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.classification_report.html) which returns P/R/F-Score per topic/class.
* What could you do to improve the accuracy of your model?
  * Training the model(s) specifically the BERT model with more diverse training data. We only used 50k of samples to train this model, if we want to use this model in production, we definitely need to use all the training samples. Furthermore, we need to generate new diverse samples using text augmentation techniques such as paraphrasing, and summarization.
* How would you serve your model(s) in production?
  * In order to make these models ready for production environment, we need to make them available as services using REST APIs and containerization. To define and build APIs we can use frameworks like __Flask__, and to deploy models in containers we can use __Docker__.
* Imagine that there are future requirements that ask you to add a new class/topic/pattern? What would re-training look like? Are there any concerns? How would you avoid these concerns?
  * To re-train these models on new obeservations/topics/samples in future, we have three options:
    * __Online training__: each time we recieve a new sample, we train the model with it. This requires to set the __batch_size__=1 at the first time we build our model. This let the model expect only one sample (data point) per time. With this method, our model learns in a sequential manner and sort of adapts locally to the data and will be more influenced by the recent samples than by older ones. This might be useful in situations where your model needs to dynamically adapt to new patterns in data. It is also useful when you are dealing with extremely large data sets for which training on all of it at once is impossible.
    * __Offline training__: we combine new topics/samples to our previous/old dataset and retrain the model entirely. This results more generalize model with better performance and is very common if we don't have new samples to often. However, using this method when our dataset is large is challenging.
    * __Online training with Mini Batch__: we wait until we have a batch of new samples (equals to the value of __batch_size__ hyperparameter at training time) and then we train our model on this batch. This method is something in between online (with batch size=1) and offline. It is not offline as we are not adding this batch to our previous/preexisting samples and then retraining our model on it, and it is not online as we are training our model on __n__ samples at once and not just a single one. As I have set batch size equals to 16 and 64 to train CNN and BERT models respectively, I would wait and collect these amounts of new samples to train each model.
    
### Online demo
  You can try these models in [here](http://drstrange.cse.unsw.edu.au:5002)
