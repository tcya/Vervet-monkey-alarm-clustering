# Vervet_monkey_alarm_clustering
A clustering on records of vervet monkey's three types of alarms

Human’s language ability is distinctive, while animals do show some primitive speaking abilities.
The first clear evidence was found in vervet monkey, which has been found to use three types of alarms in wild, i.e., snake alarms, eagle alarms, and leopard alarms. Each alarm evokes contrasting responses. Monkeys on the ground respond to leopard alarms by running into trees, to eagle alarms by looking up, and to snake alarms by looking down [1,2].

Here I propose a methodological frame to apply machine learning to acoustic primatology.

First, we apply clustering to audio records which primatologists believe to have certain types of meaning. If the classification obtained from machine agrees to that decided by primatologists, the guess is verified.

Then we can turn to supervised learning. For example, we can use existing records to train and cross-validate an artificial network, or support vector machine. If it turns out well, we can use it to recognize new records in the wild.

Finally, the trained algorithm can be implemented in laptop, or even smart phone. Imagine a primatologist working in the wild, uses a phone to record animal's speech, then the meaning is returned, so cool..

I did a primitive study. I extracted the audio track from http://www.arkive.org/vervet/chlorocebus-pygerythrus/video-11a.html, manually truncated it to small parts containing different type of alarms. For each small record extracted:

* Audio magnitude is scaled to [-1,1];
* A fast Fourier transform applied to frames sampled by a sliding Hamming window, with 93.75% overlap. Each frame includes 1024 continuous points;
* Weak frames (average amplitude < 0.5) are discarded;
* PCA applied, with 99% variance conserved;
* Data then were clustered using Gaussian mixture model (GMM), k-means and partitioning around medoids (PAM) to 3 clusters. K-means and PAM gave satisfactory results, while hierarchical algorithm performed poorly.

Though primitive, the results are promising.

**GMM**

------------| Cluster 1 | Cluster 2 | Cluster 3
------------|------------ | ------------- | ------------
Snake  |37|144|15
Eagle  |131|8|102
Leopard|20|0|121

**K-Means**

------------| Cluster 1 | Cluster 2 | Cluster 3
------------|------------ | ------------- | ------------
Snake  |0|1|195
Eagle  |0|185|56
Leopard|101|4|36

**PAM**

------------| Cluster 1 | Cluster 2 | Cluster 3
------------|------------ | ------------- | ------------
Snake|11|0|185
Eagle|209|0|32
Leopard|13|108|20

GMM is the poorest, but it still gets >50% accuracy, better than random guess (33% accuracy). Both k-means and PAM give satisfactory results. For k-means and PAM, most of snake alarms are correctly identified, while for eagle and leopard alarms, PAM achieves at least 75% accuracy while k-means gets >70%. In all cases, snake alarms are well classified. It can be understood from our previous observation that snake alarms have significant component in high frequency region, while the others don't.

After verify primatologists' observation, I turn to supervised learning. Four models are tried. They're SVM, decision tree, AdaBoost, and Gaussian naive Bayes.

SVM fails, while the others all get >90% accuracy in both training and test sets. Decision tree and AdaBoost perform best, so I fine tune them further.

After tuned on stratified shuffled data, AdaBoost can achieve 99% accuracy on training set and 97% on test set, almost perfect.

The codes are in analysis.ipynb. There is another Mathematica version (clustering.nb, which gives slightly different results doesn't do supervised learning), together with its pdf (clustering.pdf, for those who don't have Mathematica). Also presented is presentation slides (the pptx file).

Reference

[1] Seyfarth R M, Cheney D L, Marler P. Vervet monkey alarm calls: semantic communication in a free-ranging primate[J]. Animal Behaviour, 1980, 28(4): 1070-1094.

[2] Seyfarth R M, Cheney D L, Marler P. Monkey responses to three different alarm calls: evidence of predator classification and semantic communication[J]. Science, 1980, 210(4471): 801-803.

