# Vervet_monkey_alarm_clustering
A clustering on records of vervet monkey's three types of alarms

Human’s language ability is distinctive, while animals do show some primitive speaking abilities.
The first clear evidence was found in vervet monkey, which has been found to use three types of alarms in wild, i.e., snake alarms, eagle alarms, and leopard alarms. Each alarm evokes contrasting responses. Monkeys on the ground respond to leopard alarms by running into trees, to eagle alarms by looking up, and to snake alarms by looking down [1,2].

Here I propose a methodological frame to apply machine learning to acoustic primatology. 

First, we apply clustering to audio records which primatologists believe to be have cetarin types of meaning. If the classification obtained from machine agrees to that decided by primatologists, the guess is verified.

Then we can turn to supervised learning. For example, we can use existing records to train and cross-validate an artificial network, or support vector machine. If it turns out well, we can use it to recognize new records in the wild.

Finally, the trained algorithm can be implemented in laptop, or even smart phone. Imagine a primatologist working in the wild, uses a phone to record animal's speech, then the meaning is returned, so cool..

I did a primitive study. I extracted the audio track from http://www.arkive.org/vervet/chlorocebus-pygerythrus/video-11a.html, manually truncated it to small parts containing different type of alarms. For each small record extracted:
* The sampling frequency is reduced to 22.05kHz;
* A fast Fourier transform applied to frames sampled by a sliding Hamming window, with 93.75% overlap. Each frame includes 1024 continuous points;
* Weak frames (average amplitude < 1) are discarded;
* PCA applied, with 99% variance conserved;
* Data then were clustered using hierarchical, k-means and partitioning around medoids (PAM) to 3 clusters. K-means and PAM gave satisfactory results, while hierarchical algorithm performed poorly.

Though primitive, the results are promising.

**Hierarchical**

------------| Snake | Eagle | Leopard
------------|------------ | ------------- | ------------
Cluster 1|1603|1307|981
Cluster 2|0|0|10
Cluster 3|0|0|1

**K-means**

------------| Snake | Eagle | Leopard
------------|------------ | ------------- | ------------
Cluster 1|1593|530|451
Cluster 2|10|777|13
Cluster 3|0|0|528

**PAM**

------------| Snake | Eagle | Leopard
------------|------------ | ------------- | ------------
Cluster 1|1550|349|338
Cluster 2|53|958|49
Cluster 3|0|0|605

For k-means and PAM, almost all snake alarms were correctly identified, while for eagle and leopard alarms, PAM achiebed at least 60% accuracy while k-means got >50%.

The codes were written in Mathematica (file clustering.nb). A pdf (clustering.pdf) and presentation (the pptx file) are also presented.

Reference

[1] Seyfarth R M, Cheney D L, Marler P. Vervet monkey alarm calls: semantic communication in a free-ranging primate[J]. Animal Behaviour, 1980, 28(4): 1070-1094.

[2] Seyfarth R M, Cheney D L, Marler P. Monkey responses to three different alarm calls: evidence of predator classification and semantic communication[J]. Science, 1980, 210(4471): 801-803.

