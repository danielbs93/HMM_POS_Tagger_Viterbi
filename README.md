# HMM_POS_Tagger_Viterbi
Implementing from scratch HMM with the Viterbi algorithm

# Overview of Markov Models
To exploit the sequential patterns that occur in the data, we need a way to model the correlations between the observations. Markov models use the product rule to express the joint distribution for a sequence of observations,



Assuming that the current observation only depends on the previous observation, 

![image](https://user-images.githubusercontent.com/49146507/120638893-689ae280-c479-11eb-8c38-802d45f9ad2b.png)


a first-order Markov Chain, and using d-separation property to reduce the above equation we get,

![image](https://user-images.githubusercontent.com/49146507/120638940-79e3ef00-c479-11eb-9269-9c00683f90bf.png)


We can also create higher orders of markov models in a similar manner.


Hidden Markov Models (HMM) are an extension of a mixture model, where there are various discrete multinomial latent variables that could be responsible for generating a particular observation in a sequence. The choice of picking a mixture component or hidden state for a particular observation depends on the choice of component for the previous observation. The transition probabilities are defined by this transition from previous hidden state to the current hidden state based on the observation denoted by A, and emission probabilities are defined by the conditional distribution of the observed variables  for each latent variable denoted by B. HMMs in general are not susceptible to local warping and variability in generations, making it an excellent choice for speech recognition, handwriting generation etc.

![image](https://user-images.githubusercontent.com/49146507/120639092-a992f700-c479-11eb-9dfb-c8c3d8686d5b.png)

# Decoding
For any model, such as an HMM, that contains hidden variables, the task of determining the hidden variables sequence corresponding to the sequence of observations is called decoding. More formally:

![image](https://user-images.githubusercontent.com/49146507/120639332-e5c65780-c479-11eb-995c-5bc292c84a66.png)

For part-of-speech tagging, the goal of HMM decoding is to choose the tag sequence t1 ...tn that is most probable given the observation sequence of n words w1 ...wn:

![image](https://user-images.githubusercontent.com/49146507/120639443-01316280-c47a-11eb-8c1a-409a90cad97c.png)

The way we’ll do this in the HMM is to use Bayes’ rule to instead compute:

![image](https://user-images.githubusercontent.com/49146507/120639500-11494200-c47a-11eb-86d6-32e79f2d69dd.png)

HMM taggers make two further simplifying assumptions. The first is that the probability of a word appearing depends only on its own tag and is independent of neighboring words and tags:

![image](https://user-images.githubusercontent.com/49146507/120639538-21612180-c47a-11eb-8c15-d451aed590c5.png)

The second assumption, the bigram assumption, is that the probability of a tag is dependent only on the previous tag, rather than the entire tag sequence;

![image](https://user-images.githubusercontent.com/49146507/120639579-2c1bb680-c47a-11eb-94fc-39cdfc8e1d68.png)

Following equation for the most probable tag sequence from a bigram tagger:

![image](https://user-images.githubusercontent.com/49146507/120639705-52415680-c47a-11eb-8685-51f1c9095a9d.png)


# Viterbi Algorithm
The Viterbi algorithm first sets up a probability matrix or lattice, with one column for each observation ot and one row for each state in the state graph. Each column thus has a cell for each state qi in the single combined automaton. Each cell of the lattice, vt(j), represents the probability that the HMM is in state j after seeing the first t observations and passing through the most probable state sequence q1,...,qt−1, given the HMM λ. The value of each cell vt(j) is computed by recursively taking the most probable path that could lead us to this cell. Formally,
each cell expresses the probability:

![image](https://user-images.githubusercontent.com/49146507/120639931-93d20180-c47a-11eb-81c4-c50e491e2981.png)

We represent the most probable path by taking the maximum over all possible previous state sequences max (q1,...,qt−1) . Like other dynamic programming algorithms, Viterbi fills each cell recursively. Given that we had already computed the probability of being in every state at time t −1, we compute the Viterbi probability by taking
the most probable of the extensions of the paths that lead to the current cell. For a given state qj at time t, the value vt(j) is computed as:

![image](https://user-images.githubusercontent.com/49146507/120640023-af3d0c80-c47a-11eb-8371-1bc5fedeffe0.png)

# References
  1. Daniel Jurafsky & James H. Martin. (2020), "Sequence Labeling for Parts of
Speech and Named Entities", Chapter 8.
  2. Blasiak, S.; Rangwala, H. (2011). "A Hidden Markov Model Variant for Sequence Classification". IJCAI           Proceedings-International Joint Conference on Artificial Intelligence.
