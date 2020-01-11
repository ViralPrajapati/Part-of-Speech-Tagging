# Part-of-Speech-Tagging
Problem Description:-
	Part of speech tagging is the process of parsing through a sentence and assigning correct part of speech to each word of the sentence. For this we are provided with a dataset to train our model and then make predictions on the test sentences.

Approach:-
	For performing POS Tagging we have implemented three techiniques. 
	1) Using Simple Bayes Net 
	2) Using Viterbi Algorithm 
	3) Using Gibbs Sampling
	
	For implementing this three techiniques we have used prior probability, emission probability, posterior probability, transition probability and initial probability.
	Prior Probability:  P(Si) = Probability of each tag.
						P(Si) = Count(Si)/Count(Total S)
	Emission Probability:	P(Wi|Si) = Probability of word given tag.
							P(Wi|Si) = Count(Wi,Si)/Count(Total Si)
	Posterior Probability:  P(Si|Wi) = Probability of tag given word.
							P(Si|Wi) = P(Wi|Si)*P(Si)
	Transition Probability: P(Si+1|Si) = Probability of next tag given current tag.
							P(Si+1|Si) = Count(Si,Si+1)/Count(Total Si)
	Initial Probability : P(Wi) = Probability of a sentence starting from word W.
						  P(Wi) = Count(Wi such that it is first word)/Count(Total Wi)

	Here, S = Tags
		  W = Words
		  Posterior probability is used in simple bayes net model
		  Initial probability, transition probability and emission probability are used in viterbi model
		  Posterior probability, emission probability and transition probability are used in gibbs model

	1) Simple Bayes Net: Here, we are calculating posterior probability for each word in the sentence and selecting the one with max probability. 
	For log posterior we return the sum of log of all the probabilities of the predicted tags.

	2) Viterbi Algorithm: Here, we are calculating the probability for each state, that is the word, in a forward pass and then selecting the most probable tag in the backward pass. 
	For this we take product of initial probability and emission probability for the W0 and then for W1 to Wi we take product of previous probability and transition probability. We store this probabilities in a list and in the backward pass select the one with max probabilities. 
	For log posterior we return the of log of max probabilities of the predicted tags.

	3) Gibbs Sampling: Here, first we are initializing the tags for each word in sentence as 'noun', it can be initialized to any random tags but we are using noun since it is the most occuring tag in the dataset. Then we calculate probability for each tag and use a random value to choose a tag from this list of probabilities and update this tag with the initially assigned 'noun'. We repeat this process for 1000 iterations and then return the final updated list of predicted tags containing most frequent tag for each word. 
	Now to calculate the probabilities, we take product of emission probability and prior for the W0, for W1 to Wi-1 we product of previous probability and transition probability, and for Wi we take product of previous probability, transition probability and transition probability P(S0|Si).
	For log posterior we keep track of all the probabilities of the sample and return the sum of probabilities of the tags that are returned in the updated list of predicted tags.

Results:-
	
	Attached with files.
	For bc.test = Final1.png
	For bc.test.tiny = Final2.png
	
Observations:-

	After implementing above mentioned algorithms while fine tuning the models we observed that we can imporve the accuracy by handling the words that are not in the dataset but are in the test sentence. The approach we used is, since "noun" is most frequent tag in the dataset we modified the emission probabilty function in way that if we encounter a word that is not in the dataset while calculating the probabilities tag "noun" will be given some extra weight. Even though this is extremely random approach we saw that there was considerable increase in the accuracy. But what it also did was unlike our previous results where Best was viterbi than gibbs and at last simple bayes net, the simple bayes net model started making pretty good predictions and got accuracy almost similar to that of gibbs. This results are shown below:

	Fine tuned results = Final3.png
