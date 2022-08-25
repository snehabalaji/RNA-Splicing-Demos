## Learning About Splicing & Computing 

*A beginner’s guide to RNA Splicing and deep learning, created to provide a foundation for using RNA splicing tools. This page also includes technical information on the prediction methods of Pangolin, MTSplice and SpliceAI.* 

This blog includes: 
* Overview of RNA Splicing 
* Overview of Deep Learning 
* Pangolin Model  
* MTSplice Model
* SpliceAI Model 
* Glossary 

Credits: 
Credits: 
I would like to thank the authors of the following papers for making their tools free and easily accessible. The tools discussed in this webpage are credited to the following people: 
* Pangolin: Created by Zeng, T. and Li, Y.I. The github repository can be found [here](https://github.com/tkzeng/Pangolin).
* SpliceAI: Created by Kishore Jaganathan,Sofia Kyriazopoulou Panagiotopoulou,Jeremy F. McRae,Siavash Fazel Darbandi,David Knowles,Yang I. Li,Jack A. Kosmicki,Juan Arbelaez,Wenwu Cui,Grace B. Schwartz,Eric D. Chow,Efstathios Kanterakis,Hong Gao,Amirali Kia et al.. The github repository can be found [here](https://github.com/Illumina/SpliceAI).
* MTSplice: Created by Jun Cheng, Thi Yen Duong Nguyen, Kamil J Cygan, Muhammed Hasan Çelik, William G Fairbrother, Žiga Avsec, Julien Gagneur. The github repository can be found [here](https://github.com/snehabalaji/MMSplice_MTSplice).


## Brief Overview on RNA Splicing 
*To fully utilize the splicing tools, a basic background in RNA splicing is required.* 

RNA splicing is a perplexing and captivating genetic process that gives rise to all human traits and characteristics. The entire process begins with the aim of protein synthesis with the precursor messenger RNA, pre-mRNA, which contains coding, extrons, and non-coding regions, introns. In order for the RNA to be translated into the proper corresponding protein, the introns must be removed through a process called RNA splicing. Shown below is an image of this process.
![splicing](https://cdn.kastatic.org/ka-perseus-images/3157cfd56297f8bc312c0b53bdf9dd8c09f07063.png)
This is done by the spliceosome, which can be thought of as “molecular scissors” which delicately trim and then join the intron regions in a loop; these loops of intron regions are then released from the pre-mRNA, transforming it into mRNA that then moves onward through the protein synthesis process to communicate the protein to create. 

Any small change in this precise process may result in a completely different RNA and thus resulting protein leading to potentially catastrophic effects. In some cases, a single "root" DNA can give rise to many end protein strands, a phenomenon known as alternative splicing. Thus, variants, whether hereditary or random, are of utmost interest to those studying the field. Shown below is a diagram representing alternative splicing.
![splicing](https://cdn.technologynetworks.com/tn/images/body/38518_tn_alternativesplicing_customimage-1_jp1628504770196.png)
In addition, other features can also influence RNA splicing such as the tissue it occurs in, this phenomenon is known as tissue-specific gene expression where the same genome can create various phenotypes in different tissues, thus yielding a complicated variety of features and systemic consequences from the same root genetic information. Examples include the c-src N1 exon, cancer-associated splicing of the CD44 gene and the alternative splicing cascade involved in Drosophila melanogaster sex determination [1](https://genomebiology.biomedcentral.com/articles/10.1186/gb-2004-5-10-r74). 

Previously, the sheer amount of splice sites and possibilities made it near impossible to predict the effects of variants on RNA splicing. However, the recent developments in computing, specifically deep learning, have paved the way for increasingly accurate and effective prediction tools. 

## Brief Overview on Deep learning 
*In addition to background knowledge on splicing, a foundation in deep learning methods may also prove to be useful.* 

Deep learning is a fairly new and rapidly evolving field of computing concerned with developing algorithms and programs that learn the way humans do. For example, virtual assistants and face recognition technologies often use these concepts. 
![deep learning](https://master-iesc-angers.com/wp-content/uploads/2018/04/2018-04-13_1322.png)
In specific, neural networks are often used in deep learning as they were inspired by the human brain’s biological signalling of neurons.
![layers](https://miro.medium.com/max/1400/1*3fA77_mLNiJTSgZFhYnU0Q.png)
Neural networks are made of many layers of interconnected nodes which are then stacked up on top of each other to create the most accurate prediction. Creating such a network requires a combination of biases, weights and data inputs. Here are some notable layers: 
* **Input Layer**: where the data, for training or testing, is fed into the network. 
* **Output Layer**: where the final prediction is created.
* **Activation Layer**: where the weighted input is interpreted as potential outputs. 

In addition layers can be visible or hidden. 
* **Hidden Layers**: layers between the input and output whose node weightage is unknown. Each neural Network has at least one hidden layer. 
* **Visible Layers**: layers which are visible, in terms of node weightages, to the user such as the input and output layers.  

Using these different layers and mathematical formulas, the network interprets the input, in our case a set of genes, to make a prediction, RNA splicing for example.  

## Pangolin 
### Architecture 

Pangolin is a deep learning technique that uses DNA sequences from four species: humans, Rhesus monkey, rats and mouse (with 8 samples from four different tissues: brain, testis, heart and liver) to predict both usage of a splice site and probability with reasonable accuracy in genetic variations. Pangolin has also been shown to have a higher AUPRC than other pre-existing models. This model can also detect the effects of rare variants as well as account for epistatic effects. However, its tissue-specific prediction abilities are low but higher than MTSplice. 

Pangolin’s model consists of a neural network with 16 stacked residual blocks, created from ReLU activation, convolutional and batch normalization layers. Along with the residual block layer, Pangolin contains three other layers: the first, the penultimate and an activation layer at the very end that helps to produce the final probability or usage output. Pangolin’s model allows for genetic features to be referenced to a maximum of 5000 base pairing upstream and downstream of the target genome. An input of one-hot N encoded base sequences is fed into the model to generate the probability and usage predictions; depending on the usage, N is adjusted. For example, N must be greater or equal to 10, 001 in order to predict 10, 000 bases and for individual site predictions, N must be 10, 001. 

In terms of pre-processing the genetic data, genetic information collected from the samples are then labelled with two labels: first, a binary label as spliced or not spliced and a second label as the usage of each site. Then, SpliSER was used to gain RNA splicing measurements and usage. After, the training set was split from genomic positions from genes on human chromosomes 2, 4, 6, 8, 10-22 (X and Y), including splice and non-splice sites; the test set was chromosomes 1, 3, 5, 7, and 9. It is important to note that for the additional species, genes that were not orthologs or paralogs (to the human genes) were used. 

Then, the models were tested on both splice site usage and rare variant effect predictions. For rare variant predictions, MFASS (a Sort-seq assay, single cell sequencing platform) was used to generate data for exonic (creates a nucleotide that can cause an amino acid substitution) and intronic (affects splice site recognition) variants from ExAC (Exome Aggregation Consortium). In addition, to predict pairwise and more complicated epistatic relationships between variants and splicing results, PSI (percent-spliced-in) on exon 6 of FAS (Fas Cell Surface Death Receptor) gene with single or multiple nucleotide substitutions was used. 

Tissue specific usage was tested by calculating differences between estimated splice site usage in specific tissue and average usage across tissues for each site on test chromosome genes; this result was then compared to Pangolin’s predictions with analysis being limited to sites which had a difference in usage greater than 0.2. 

## MTSplice 
### Architecture
MMSplice (Modular Modeling of Splicing) and MTSplice are neural networks that predict RNA splicing using cassette exons instead of splice sites. MTSplice is created by running MMSplice’s predictions through TSplice which has tissue specific splicing to create predictions on various tissues.  

The fundamental idea behind the deep learning approach for this technique is to model the chances that a cassette exon will be spliced by comparing it to an alternative sequence. To train TSplice, 56 human tissues were used in multi-task learning to gain tissue-specific capabilities. With more specific reference to the idea behind the model, it quantifies the percent spliced-in (PSI or 𝛹) which represents exon skipping and is defined as the fraction of different transcripts that contain a specific exon. Similarly, 𝛹5 is defined as the percent of spliced RNA from a specific donor site with a specific alternative 3’ site whereas 𝛹3 is for alternative 5’ sites.  

To build the neural network, separate models were trained for the exon, the acceptor site, the donor site, the exon and for intronic areas in close proximity to the donor and acceptor sites (5’ end and 3’ end). This was done so that multiple datasets could be used to train the model from two primary sources: MPRA (massively parallel reporter assay) with random short sequences and a high-throughput assay that uses naturally occurring exonic variants. The intron and exon models were trained from a MPRA using the results of millions of randomized sequences alternating between the 3’ exon or the 5’ intro and the alternative splice sites. The donor layer consisted of a multilayer perceptron and two hidden layers with Rectified Linear Unit (ReLU). These hidden layers had a dropout rate of 0.2 and batch normalization. The acceptor model was a convolutional neural network with two convolution layers with the second having a dropout rate of 0.2 and batch normalization. 

Similar to Pangolin, a one-hot-encoded input method was used to train the model. The training database was composed of positive, annotated sites and negative, random sites for both donor and acceptor sites. The positive set was created using GENCODE version 24 with annotations. The negative set was made from sequences with genes that contributed to positive splice sites with 50% of the sites  having canonical splicing dinucleotides to increase the breadth of the set. 
The resulting architecture is two intron models (one from the 5’ donor end and the other from the 3’ acceptor end) and two exon models (3’ and 5’). Thus, these models are combined to predict differences in 𝛹3, 𝛹5, and 𝛹 and then compare them to disease phenotypes referenced from the ClinVar database. In addition, two linear models were trained: one to predict Δ𝛹5 and Δ𝛹5 using a model of the completed alternative exon and one to predict the change in splicing efficiency. 


## SpliceAI
### Architecture
SpliceAI predicts using a neural network made of 32 dilated convolutional layers that can recognize sequences through large genetic distances. To create both the training and testing dataset, human chromosomes were annotated using GENCODE and genes without splice junctions were removed. Then, the training dataset was constructed from chromosomes 2, 4, 6, 8, 10-22, and X and Y with the rest being used for testing. In addition, 10% of the testing set was randomly chosen to be the point for early-stopping during training with the rest used to purley train the network. 

With reference to SpliceAI’s architecture, four different models were created: SpliceAI-80nt, SpliceAI-400nt, SpliceAI-2k and SpliceAI-10k which each utilize 40, 200, 1,000 and 5,000 nucleotides from either side of a specific area on the pre-MRNA as input. The genetic information was then one-hot encoded with A, C, G and T represented as [1, 0, 0, 0], [0, 1, 0, 0], [0, 0, 1, 0] and [0, 0, 0, 1]. Then, the output would be the probability of that site acting as an acceptor or donor in the form of three values (which add up to one), each one representing the probability of that site being an acceptor, donor or neither. 

These encoded data sets were trained for 10 epochs with a batch size of 12 with a learning rate of 0.001 for the first 6 epochs and then reduced by a factor of 2 for each epoch afterwards. For each model, the training process was recreated 5 times for each model. During the testing phase, every input was fed through all 5 models and the average of their outputs was taken to be the final output. 

SpliceAI’s network was created from residual blocks which had batch-normalization layers, ReLUs and convolutional units; these layers are then arranged with K stacked residual blocks that connect to both the input and penultimate layer with a CNN layer (with softmax activation) between the penultimate layer and the output layer. Also, skip connections were added with every fourth residual block being connected to the penultimate layer to increase convergence speed when training. Each residual block consisted of three hyper-parameters: N (the number of convolutional kernels), W (the window size) and D (the dilation rate of each convolutional kernel). 

## Glossary 
* Gene Transfer File (GTF): a type of file formatting that is used for storing information about genetic structure. 
* Neural Networks: computing algorithms modelled after human neural connections in the brain. 
* Deep Learning: a field of computing concerned with developing algorithms and programs that learn the way humans do
* Area Under the Precision Recall Curve (AUPRC): Known as AUPRC is a metric for binary responses used to measure the prediction accuracy of a technology.
* Cassette Exons: intervening exons that can be excluded or skipped and generate distinct protein results. 
* Pre-mRNA: Precursor messenger RNAs (or Pre-mRNA) communicate information from DNA to protein synthesizing units abou how to build the target protein. 
* Alternative Splicing: different splice sites that allow for a single parent gene to code for many different proteins. 
* Weights: In the context of deep learning weights are numerical values that represent the strength of the connection between two neurons. 
* Nodes: a computational unit made of an input, transfer connection and output.

## References 
* Zeng, T., Li, Y.I. Predicting RNA splicing from DNA sequence using Pangolin. Genome Biol 23, 103 (2022). https://doi.org/10.1186/s13059-022-02664-4 
* Kishore Jaganathan, Sofia Kyriazopoulou Panagiotopoulou, Jeremy F. McRae, ..., Serafim Batzoglou, Stephan J. Sanders, Kyle Kai-How Farh. “Predicting splicing from primary sequence with deep learning”. Cell. (n.d.). Retrieved June 20, 2022, from https://www.cell.com/cell/fulltext/S0092-8674(18)31629-5 
* Cheng, J., Çelik, M.H., Kundaje, A. et al. MTSplice predicts effects of genetic variants on tissue-specific splicing. Genome Biol 22, 94 (2021). https://doi.org/10.1186/s13059-021-02273-7
