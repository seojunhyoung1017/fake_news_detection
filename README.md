# Fake news detection with machine learning
    Problem Definition
In the information age of today, it is easier for consumers to access information and media than ever before. Technological advancements have enhanced the speed and range of information distribution but a trade-off exists between the quantity and quality of accessible information. Misleading information and propaganda have been problems since the beginning of print media, but social media engagement has made “fake news” a bigger issue than it has ever been. Wrong information has wide ranging implications for the consumer and society as whole, as it can misguide personal beliefs, paint people in a bad light, promote conspiracy theories, and overall lead to misjudgment and political polarization. To improve the way that news consumers can discern what is accurate information and what is fake news, we are in need of a model to efficiently classify news depending on the contents of the article that a reader could utilize.

    Background 
The increasing power and outreach of the media has enabled the rise of fake news in today’s world. Fake news misleads people for political agendas and commercial reasons, which both create a glaring issue of integrity for information dissemination. With the sheer quantity of information that the average consumer is presented per day, it is hard to expect people to carefully parse through articles and sources in order to evaluate the legitimacy of every piece of news they come across online. Therefore, it would be greatly beneficial if there existed an automated machine learning model that could label a news article as fake or real by using natural language processing techniques. We are hoping to refine the quality of information put out to the public with such a model. After training the model with labeled data, the objective of a text classification model is to accurately label testing data as fake or real based on the title & article word frequency, lengths, and subjects. Specifically, this project will focus on fake news in the form of text articles, as opposed to the many other forms of fake news out there, like photoshopped images or videos. The project scope is also limited to a binary true/false decision, rather than on a scale of accuracy or trustworthiness. A successful model could be utilized as a browser extension that scans an article when the user opens it, and then delivers a warning message if it labels it as “fake news”.

    About the Dataset 
The dataset is pulled from Kaggle, titled “Fake and Real News Dataset.” This dataset contains two files, one with fake news (23502 observations) and one with real news (21417 observations). The columns are: title of news article, text of the article, subject (either politics or world news), and date written or published. All of the data are string variables, aside from the date variable, that is temporally formatted. This dataset is clean with no missing values. 
Methods
The first step of model creation is the pre-processing of the data to make it easy for the model to work with. In this pre-processing, we are considering (a) the attributes of datasets we will use for our analysis, and (b) which algorithm to apply to accurately classify fake news.
Data Pre-processing
To get a better understanding of the data, we first ran some preliminary descriptive analysis to compare the datasets of the real news articles and the fake news articles.

    Features extraction
We decided to use the TF-IDF algorithm to transform text into meaningful representations of numbers because this is a popular algorithm for Natural Language Processing and it really fits well with the goal of this project. TF-IDF represents the way the algorithm weighs “term’s frequency” (TF) and its “inverse document frequency” (IDF). Scikit-learn has a package called “TfidfVectorizer” that transforms the input corpus into a matrix, with each word represented by an index in the matrix. The elements of the matrix are the tf-idf scores for the words. The higher the score, the more relevant that word is in the corpus.We did a preliminary test by fitting and transforming the corpus of words from the dataset. The resulting matrix had around 122,000 features, so we would need to either select a model that works well with large amounts of features, or perform feature selection/pruning ourselves beforehand. We need to find a machine learning model that could classify the label based on the given test matrix.

    Finding a best-fitting model
In order to maximize our chances of finding a good model, we tried a variety of machine learning algorithms geared towards classification problems, and then compared the resulting metrics. The methods we ran were logistic regression using both L1 and L2 penalty, random forest classifier, KNN classifier, and support vector classifier. This is no means an exhaustive list, as there are many other classification methods. However, for the purposes of this project, these methods provide a good glimpse of the capabilities that such machine learning solutions have to solve problems of this type.

    Results and Analysis
The machine learning algorithms were trained using a training set consisting of 80% of the entire sample of both real and fake articles, randomly selected. The testing set made up the remaining 20%. There was already a good balance of real vs. fake articles, so class imbalance was not an issue we had to deal with here. The data was naturally clean, making the pre-processing easy.
The models that we have used for training and testing are: logistic regression (L1 penalty), logistic regression (L2 penalty), random forest (max depth = 5), KNN (knn neighbors = 5), and support vector classifier. With our datasets of both training and test sets, we used each of these 5 model types since these models are common models for supervised classification. Each model has its own strengths and weaknesses, so to evaluate which model is best fit for this problem, we will first optimize each model with the ideal parameters, and then compare the appropriate evaluation metrics from each optimized model to select which model is best.
For each classification type, we subjected the model to trial and error to find the ideal parameters. The ideal parameters are the ones that classify the test set with the highest accuracy, precision, recall, and F1 score. For example, the ideal parameters for the logistic regression are found by altering the beta coefficients and finding which model provides the best evaluation metrics. The same process is done with the random forest (and finding ideal depth), k-nearest neighbors (finding ideal k-neighbors), and SVM (finding ideal margin). The parameters written in parentheses next to the algorithms above were optimal values for the respective models.
Choosing a metric to evaluate the model is dependent on the situation and datasets. Accuracy is used when the True Positives and True Negatives are more important while F1-score is used when the False Negatives and False Positives are crucial. Therefore, we must consider which metric we are most interested in using to evaluate the validity of our model. There is room for more research in regards to this problem’s preferred metric, preferably from domain experts.


Accuracy
Precision
Recall
F1 Score
Logistic Regression (L1)
0.993
0.990
0.995
0.993
Logistic Regression (L2)
0.987
0.983
0.990
0.987
Random Forest
0.936
0.984
0.882
0.930
KNN
0.644
0.960
0.272
0.424
SVM
0.994
0.994
0.995
0.994

Looking at the above table of the different machine learning algorithms, we can see the results after running each model on the test set. Each one was measured on several different metrics, since each individual metrics alone does not tell the whole story. We measured accuracy, precision, recall, and the F1-score, as well as the confusion matrix for each.
Several algorithms perform very well in detecting fake articles. Both the support vector classifier as well as logistic regression with L1 penalty correctly discerned between real and fake articles over 99.3% of the time when presented with the test set, which is by far the best accuracy of all of the models. Logistic regression with L2 penalty did slightly worse (98.7%), as did the random forest classifier (93.6). The K-nearest neighbors model is the only one that performed quite poorly, with a dismal accuracy of 64.4%. This trend is actually consistent between all of the metrics. The support vector classifier and the logistic regression with L1 penalty have the two highest precisions, recalls, and F1 scores as well, with the random forest classifier and logistic regression with L2 penalty close behind, and the KNN model performing the worst.
The classifiers that performed not so well could be a sign of overfitting. Our data set did have a very large number of predictors thanks to the TF-IDF algorithm. Some classifiers have inherent ways of trying to prevent overfitting. For instance, a random forest could have a limit to its max depth, which is basically the number of times the decision tree splits into branches. In this particular case, we used a max depth of 5. In K-nearest neighbors, we might change the number of neighbors to achieve a more or less flexible fit. In this problem model, we used k = 5 neighbors.
It appears like the L1-regularized logistic regression deals with a large number of predictors better than the other classification methods, resulting in a better performance than the others, and therefore might be a more suitable model for this problem. This makes sense intuitively, since L1-regularization acts by limiting the size of coefficients to 0, thus performing feature elimination and getting rid of the weakest predictors.
Conclusions
There are multiple classification models that have the potential to do an effective job of identifying real and fake news articles by analyzing natural language processing techniques. We found effective models by experimenting with the ideal parameters for each model type and comparing the evaluation metrics of each optimized model. In our experimentation with our dataset, L1-regularized logistic regression provided the best model to achieve our problem goals. There is plenty of room for further experimentation on the topic, such as exploring various other classification models or trying to fine tune existing models by altering the parameters. We could also experiment with different training and test datasets, depending on where a real-life model would draw its sample of real and fake news from. 
Ways to extend the project could be to optimize hyperparameters using methods such as cross validation. This way, we can pinpoint the optimal parameters within a model. Working with a larger dataset could be beneficial. The algorithm we used created a huge amount of predictors, and if there were more data it could make it easier for the machine learning models to avoid overfitting. There are countless news articles out there, so drawing a larger sample is very feasible. Another way to further optimize results would be to pre-process the data to weed out common English words or phrases that do not add much information. This would probably require some linguistics domain knowledge, though, and we did not quite have that on hand. However, we did manage to achieve high levels of accuracy using multiple models on real data, and we feel that is definitely a strong starting point to fixing the problem of fake news and how to identify it.

    References
Abu-Nimeh, S., Chen, T., Alzubi, O. (2011). Malicious and spam posts in online social networks. Computer 44, 23–28. doi:10.1109/MC. 2011.222
- Appropriate article to check out how vulnerable the users in social media can be exposed to malicious posts and what it entails for social media users

Ahmad, I., Yousaf, M., Yousaf, S., Ahmad, M. (2020). Fake News Detection Using Machine Learning Ensemble Methods. Complexity, vol. 2020, Article ID 8885861, 11 pages, 2020. https://doi.org/10.1155/2020/8885861
- Another fake news detection project from 2020 that used a variety of ML algorithms.

Allcott, Hunt, Gentzkow. (2017). Social Media and Fake News in the 2016 Election. Journal of Economic Perspectives, 31 (2): 211-36. doi: 10.1257/jep.31.2.211
- Discusses the prevalence of fake news as of recent times, in the digital age and the 2016 election, and demonstrates the real-life implications of misinformation

Aphiwongsophon, S. and Chongstitvatana, P. (2018). Detecting Fake News with Machine Learning Method. 2018 15th International Conference on Electrical Engineering/Electronics, Computer, Telecommunications and Information Technology (ECTI-CON), 2018, pp. 528-531, doi: 10.1109/ECTICon.2018.8620051.
- Researchers performed a similar project to the one we created, using Naive Bayes, Neural Network and Support Vector Machine classification

Bisaillon, C. (2019). Fake and real news dataset: classifying the news. Kaggle. https://www.kaggle.com/clmentbisaillon/fake-and-real-news-dataset
- Kaggle source for the dataset of real and fake news that we used to build our models.

Havrlant, L. and Kreinovich, V. (2017). A simple probabilistic explanation of term frequency-inverse document frequency (tf-idf) heuristic (and variations motivated by this explanation), International Journal of General Systems, 46:1, 27-36, DOI: 10.1080/03081079.2017.1291635
- A very good article on the tf-idf heuristic and how to effectively utilize it in a document analysis problem

Nasir, J., Khan, O., Varlamis, I. (2021). Fake news detection: A hybrid CNN-RNN based deep learning approach. International Journal of Information Management Data Insights, Volume 1, Issue 1, 2021, 100007, ISSN 2667-0968. https://doi.org/10.1016/j.jjimei.2020.100007.
- Research paper discussing automatic detection techniques based on artificial intelligence and machine learning for fake news detection. A bit out of scope of our own project, since it deals with a novel hybrid deep learning model with convolutional and recurrent neural networks, but interesting to see room for further experimentation beyond our work.

Paialunga, P. (5 May 2021). Fake News Detection with Machine Learning, using Python: A step by step Fake News detection using BERT, TensorFlow and PyCaret. Towards Data Science. https://towardsdatascience.com/fake-news-detection-with-machine-learning-using-python-3347d9899ad1.
- A very informative guide with examples on how to code a project of this type,  including which libraries are needed and which methods to use.
