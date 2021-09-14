# Distributed Novelty Detection at the Edge for IoT Network Security at ICCSA 2021

By Luís Puhl, Guilherme Weigert Cassales, Helio Crestana Guardia, Hermes Senger.
Universidade Federal de São Carlos, Brasil.

ICCSA 2021, Session 9c. General Track.
September 14, 2021, 15:15 (GMT+02:00) Central European Time.
Cagliari, Sardinia, Italy.

## 1 - Title

>Hello everyone, I am Luís Puhl, from the Universidade Federal de São Carlos, Brasil.\
I am here to present our work on Distributed Novelty Detection at the Edge for IoT Network Security.

time: 15s

## 2 - Introduction

>Our main motivation is the growth of IoT and associated risks in recent years, such as the record breaking Denial of Service attacks from IoT botnets.\
To address such risks, network intrusion detection systems, especially those detecting anomalies, need to work well in IoT and Fog scenarios.\
To that end, we chose a novelty detection algorithm to be ported to a fog scenario intending to have a small detection latency without quality degradation.

time: 35s

## 3 - Introduction - Scenario

>Our scenario is a common IoT scenario where sensors at the edge communicate via gateways to the public internet.\
Traditionally, the risk is at the edges and power to detect such risks is in the cloud.\
We aim to reduce detection latency by running the system on small fog computers nearest to the data source.

time: 23s

## 4 - Related Work

>Previous work on this subject includes Bigflow, a system that spans from network flow descriptor extraction to alarm generation, however this system requires external updates and is cloud only.\
Another inspiring work is catraca. It also handles flow extraction to alarm generation and, by using network virtualization functions, can run probes in the fog, however detection is done in cloud.

time: 33s

## 5 - Related Work

>As another approach, there is the IDSA-IoT Architecture, where three novelty detection algorithms are evaluated in a fog and iot context.\
However, this architecture does not implement a full system, handling large data.

time: 19s

## 6 - MINAS Algorithm

>With the state of the art explored, we present the chosen algorithm, MINAS.\
Minas is a offline-online novelty detection algorithm that takes a flow of examples and gives examples labeled as known, extension of a known class, novelty or unknown.\
To that end, minas uses a list of spherical clusters as model and euclidean distance for the classification task.\
It also uses k-means and CluStream as basis to find new patterns.

time: 40s

## 7 - Proposal

>Our proposal is to employ MINAS in a Intrusion Detection role in a distributed fog environment, following the IDSA-IoT architecture.\
We also Observe the effects on classification quality due to the change from sequential to a distributed algorithm.\
As for our method, we chose a technique and platform for the distributed implementation and in the end we increased fog usage in the architecture to minimize latency and also due to technology constraints.\
[from flow description intake and classification to novelty detection]\
For validation, our experiments were designed with a suitable environment and dataset.\
Also metrics were chosen for quality and scalability assertion.

time: 43s\
208s 3.4min

## 8 - Implementation with MPI

>Our implementation uses OpenMPI and is architected in two modules: root and leaf.\
One node runs the root module and the remainder nodes in the cluster run the leaf module.\
The root module has two tasks: sampler and detector. While the leaf has the classifier and model receiver tasks.

time: 19s\
227s

## 9 - Implementation with MPI

>Following the sequence in the diagram:\
The Sampler task at the root takes the input data and distributes via round robin to each leaf while the Classifier task is ran distributedly in the leafs.\
This Classifier takes in a sample and the current model and assigns a cluster’s label, if found, sending the result to the detector.\
The Detector task takes in the labeled samples from the leafs and puts forth this result to the applications output [for the next user or system].\
The Detector task also stores the samples labeled as unknown to execute the novelty detection method when needed and then broadcasts the new clusters to the Model Receiver task on the leafs.\
The Model Receiver task is used to maintain the leaf model up to date asynchronously.

time: 58s\
285s, 4.75min

## 10 - Experimental Setup

>In our experiments, our setup was a 3 Raspberry Pi 3B cluster over Ethernet and the December 2015 segment of the Kyoto 2006+ IDS data set.\
This data set was treated and then split in 10% for training with only normal class and 90% for test with majority attack class.\
We choose this distribution to highlight the novelty detection aspect as our goal is to detect never seen before attacks.\
For the quality measurement we used the Multiclass confusion matrix with novelty label assignment and its derived metrics.\
And for scalability we used simple GNU Time measurements as we have a fixed size test dataset.

time: 49s, 334s, 5.5min

## 11 - Validation

>So this is the stream visualization for the reference Minas implementation and our new development, both in sequential operation.\
In this visualization novelties are shown as vertical dashed lines with some labels shown at the top.\
Those lines shows us that our implementation catches less novelties, 9 instead of the previous 12.\
In this visualization for each sample in the output, the confusion matrix and derived summary metrics are updated and presented as de horizontal curves.\
Those curves shows us that there is discrepancy in the implementations but the observed metrics are roughly the same.

time: 47s, 381s, 6.35min

## 12 - Distribution

>For the parallel and distributed versions of the experiment, fewer novelties were detected but we see no significant change in the summary metrics.\
The main effect we want to draw attention to, is the shift in the novelty label “0” from the 300k mark to the 400k mark.\
This shift was our main concern when developing an asynchronous novelty detection task.\
As the main classifiers still working, the model is updated and until the proper propagation of novel cluster, those classifiers are producing results with a incomplete model.\
This also shows one behavior of the Minas algorithm were samples used to form new clusters are discarded even if they could be presented with a more accurate label at the cost of duplicated output samples.

time: 60s, 441s, 7.35min

## 13 - Measures Summary

>Finally this is the summary of our main experiments.\
The main takeaways here are the speedup, where we had a maximum of 0.97 with 4 nodes and 12 processors.\
[This speedup is low but can be improved with more nodes as done in the literature.]\
And the time taken, were the reference took 2700 seconds and our tests took around 100 seconds to handle the same dataset.

time: 35s, 476s, 6.35min

## 14 - Behavior

>This is the overview of time usage on the root node per added cpu instance.\
Here we can see that the user and system metrics trend towards each other at 200 seconds as we add instances.\
We reason that this is due to computation versus communication limit in our implementation.

time: 23s, 499s, 6.35min

## 15 - Conclusion

>In conclusion, Data Stream Novelty Detection as in Network Intrusion Detection or system behavior monitoring and analysis on IoT environments is still challenging due to data volume, latency and small devices constraints.\
This approach has some success in handling the intrusion detection in this constrained scenario.\
Distribution caused an impact on the quality measures and mitigation of those impacts must be included in further attempts.\
Our new implementation was faster than the reference one however good speedup was not found.\
Further work can be done with other novelty detection algorithms or by changing minas internals.\
Other line of work can be done on improving communication-to-computation or load balancing.

time: 61s, 460s, 8.33min
