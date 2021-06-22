---
title: 'General ML concepts'
date: 2017-11-19
#modified: 
permalink: /machine-learning-glossary/concepts
toc: true
excerpt: "ML concepts: general."
header: 
  teaser: "blog/glossary/glossary.png"
tags:
  - ML
  - Glossary
redirect_from: 
  - /posts/2017/11/glossary-concepts/
author_profile: false
sidebar:
  title: "ML Glossary"
  nav: sidebar-glossary
---

{% include base_path %}

## Curse of Dimensionality
The Curse of dimensionality refers to various practical issues when working with high dimensional data. These are often computational problems or counter intuitive phenomenas, coming from our Euclidean view of the 3 dimensional world. 

They are all closely related but I like to think of 3 major issues with high dimensional inputs $x \in \mathbb{R}^d, \ d \ggg 1$:

### Sparsity Issue
 You need exponentially more data to fill in a high dimensional space. *I.e.* if the dataset size is constant, increasing the dimensions makes your data sparser. 

 :bulb: <span class='intuition'> Intuition </span> : The volume size grows exponentially with the number of dimensions. Think of filling a $d$ dimensional unit hypercube with points at a $0.1$ interval. In 1 dimension we need $10$ of these points. In 2 dimension we already need 100 of these. In $d$ dimension we need $10^d$ observation !

<div class="exampleBoxed">
<div markdown="1">
Let's look at a simple <span class='exampleText'> example </span>:

Imagine we trained a certain classifier for distinguishing between :white_circle: and :large_blue_circle:. Now we want to predict the class of an unkown observation :black_circle: . Let's assume that: 
 * All features are given in percentages $\[0,1\]$
 * The algorithm is [non-parametric](#parametric-vs-non-parametrics){:.mdLink} and has to look at the points in the surrounding hypercube, which spans $30\%$ of the input space (see below).

Given only 1 feature (1D), we would simply need to look at $30\%$ of the dimension values. In 2D we would need to look at $\sqrt{0.3}=54.8\%$ of each dimensions. In 3D we would need $\sqrt[3]{0.3}=66.9\%$ of in each dimensions. Visually:

<div class="col-sm-4" markdown="1">
![sparsity in 1D](/images/blog/glossary-old/hDimension-sparsity-1.png)
</div>

<div class="col-sm-4" markdown="1">
![sparsity in 2D](/images/blog/glossary-old/hDimension-sparsity-2.png)
</div>

<div class="col-sm-4" markdown="1" >
![sparsity in 3D](/images/blog/glossary-old/hDimension-sparsity-3.png)
</div>

.

In order to keep a constant support (*i.e.* amount of knowledge of the space), we thus need more data when adding dimensions. In other words, if we add dimensions without adding data, there will be large unknown sub-spaces. This is called sparsity.

I have kept the same number of observation in the plots, so that you can appreciate how "holes" appear in our training data as the dimension grows. 
</div>
</div>

 :x: <span class='disadvantage'> Disadvantage </span> : The data sparsity issue causes machine learning algorithms to fail finding patterns or to overfit.

### Points are further from the center
Basically, the volume of a high dimensional orange is mostly in its skin and not in the pulp! Which means expensive high dimensional juices :pensive: :tropical_drink:

:bulb: <span class='intuition'> Intuition </span> : The volume of a sphere depends on $r^d$. The skin has a slightly greater $r$ than the pulp, in high dimensions this slight difference will become very important.

If you're not convinced, stick with my simple proof. Let's consider a $d$ dimensional unit orange (*i.e.* $r=1$), with a skin of width $\epsilon$. Let's compute the ratio of the volume in the skin to the total volume of the orange. We can avoid any integrals by noting that the volume of a hypersphere is proportional to $r^d$ *i.e.* : $V_{d}(r) = k r^{d}$. 

$$
\begin{align*} 
ratio_{skin/orange}(d) &= \frac{V_{skin}}{V_{orange}} \\
&= \frac{V_{orange} - V_{pulp}}{V_{orange}} \\
&= \frac{V_{d}(1)  - V_{d}(1-\epsilon) }{V_{d}(1)} \\
&= \frac{k 1^d - k (1-\epsilon)^d}{k 1^d} \\
&= 1 - (1-\epsilon)^d
\end{align*} 
$$

Taking $\epsilon = 0.05$ as an example, here is the $ratio_{skin/orange}(d)$ we would get:


<div class="col-sm-3 col-xs-6" markdown="1">
![2D orange](/images/blog/glossary-old/orange-2D.png)

$$9.8 \%$${:.centerContainer}
</div>

<div class="col-sm-3 col-xs-6" markdown="1">
![3D orange](/images/blog/glossary-old/orange-3D.png)

$$14.3 \%$${:.centerContainer}
</div>

<div class="col-sm-3 col-xs-6" markdown="1">
![5D orange](/images/blog/glossary-old/orange-5D.png)

$$22.6 \%$${:.centerContainer}
</div>

<div class="col-sm-3 col-xs-6" markdown="1">
![10D orange](/images/blog/glossary-old/orange-10D.png)

$$40.1 \%$${:.centerContainer}
</div>


<font color="white">.</font>

:mag: <span class='note'> Side Notes </span> : The same goes for hyper-cubes: most of the mass is concentrated at the furthest points from the center (*i.e.* the corners). That's why you will sometimes hear that hyper-cubes are "spiky". Think of the $\[-1,1\]^d$ hyper-cube: the distance from the center of the faces to the origin will trivially be $1 \ \forall d$, while the distance to each corners will be $\sqrt{d}$ (Pythagorean theorem). So the distance to corners increases with $d$ but not the center of the faces, which makes us think of spikes. This is why you will sometimes see such pictures : 


<div class="col-xs-4" markdown="1">
![2D hypercube](/images/blog/glossary-old/hypercube-2D.png)
</div>

<div class="col-xs-4" markdown="1">
![3D hypercube](/images/blog/glossary-old/hypercube-3D.png)
</div>

<div class="col-xs-4" markdown="1" >
![7D hypercube](/images/blog/glossary-old/hypercube-7D.png)
</div>

### Euclidean distance becomes meaningless
There's nothing that makes Euclidean distance intrinsically meaningless for high dimensions. But due to our finite number of data, 2 points in high dimensions seem to be more "similar" due to sparsity and basic probabilities.

:bulb: <span class='intuition'> Intuition </span>:
* Let's consider the distance between 2 points $\mathbf{q}$ and $p$ that are close in $\mathbb{R}^d$. By adding independent dimensions, the probability that these 2 points differ greatly in at least one dimension grows (due to randomness). This is what causes the sparsity issue. Similarly, the probability that 2 points far away in $\mathbb{R}$ will have at least one similar dimension in $\mathbb{R}^d, \ d'>d$, also grows. So basically, adding dimensions makes points seem more random, and the distances thus become less useful.
* Euclidean distance accentuates the point above. Indeed, by adding dimensions, the probability that $\mathbf{x}^{(1)}$ and $\mathbf{x}^{(2)}$ points have at least one completely different feature grows. *i.e.* $\max_j \, (x_j^{(1)}, x_j^{(2)})$ increases. The Euclidean distance between 2 points is $D(\mathbf{x}^{(1)},\mathbf{x}^{(2)})=\sqrt{\sum_{j=1}^D (\mathbf{x}_j^{(1)}-\mathbf{x}_j^{(2)})^2}$. Because of the squared term, the distance depends strongly on $max_j \, (x_j^{(1)}-x_j^{(2)})$. This results in less relative difference between distances of "similar" and "dissimilar points" in high dimensions. Manhattan ($L_1$) or fractional distance metrics ($L_c$ with $c<1$) are thus preferred in high dimensions. 


In such discussions, people often cite a [theorem](https://www.researchgate.net/profile/Jonathan_Goldstein4/publication/2845566_When_Is_Nearest_Neighbor_Meaningful/links/09e4150b3eb298bf21000000/When-Is-Nearest-Neighbor-Meaningful.pdf){:.mdLink} stating that for *i.i.d* points in high dimension, a query point $\mathbf{x}^{(q)}$ converges to the same distance to all other points $P=\\{\mathbf{x}^{(n)}\\}_{n=1}^N$ :

$$\lim_{d \to \infty} \mathop{\mathbb{E}} \left[\frac{\max_{n} \, (\mathbf{x}^{(q)},\mathbf{x}^{(n)})}{\min_{n} \, (\mathbf{x}^{(q)},\mathbf{x}^{(n)})} \right] 
\to 1$$

:wrench: <span class='practice'> Practical </span>  : using [dimensionality reduction](#dimensionality-reduction){:.mdLink} often gives you better results for subsequent steps due to this curse. It makes the algorithm converge faster and reduces overfitting. But be careful not to underfit by using too few features.

:mag: <span class='note'> Side Notes </span>  : 
* Although the curse of dimensionality is a big issue, we can find effective techniques in high-dimensions because:
  * Real data is often confined to a lower *effective* dimensionality (*e.g.* a low dimensional manifold in a higher dimensional space). 
  * Interpolation-like techniques can overcome some of the sparsity issues due to the local smoothness of real data.
* You often see plots of the unit $d$-ball volume vs its dimensionality. Although the non-monotonicity of [such plots](http://bit-player.org/2011/the-n-ball-game){:.mdLink} is intriguing, they can erroneously make you believe that high dimensional hypersphere are smaller than low dimensional ones. This does not make sense as a lower dimensional hypersphere can always be fitted in a higher dimensional one. The issue arises from comparing apple and oranges (no puns intended :sweat_smile:) due to different units: Is $0.99 m^2$ really smaller than $1 m$?

:information_source: <span class='resources'> Resources </span> : Great post about the [curse of dimensionality in classification](http://www.visiondummy.com/2014/04/curse-dimensionality-affect-classification/){:.mdLink} which inspired me, [On the Surprising Behavior of Distance Metrics in High Dimensional Space](https://bib.dbvis.de/uploadedFiles/155.pdf){:.mdLink} is a famous paper which proposes the use of fractional distance metrics, nice [blog](https://martin-thoma.com/average-distance-of-points/#average-angle){:.mdLink} of simulations.

Images modified from: [oranges](https://design.tutsplus.com/tutorials/how-to-make-a-delicious-vector-orange-in-9-decisive-steps--vector-229){:.mdLink}, [7D cube](http://yaroslavvb.blogspot.sg/2006/05/curse-of-dimensionality-and-intuition.html){:.mdLink}
## Evaluation Metrics
### Classification Metrics
#### Single Metrics
{:.no_toc}

:mag: <span class='note'> Side Notes </span> : The focus is on binary classification but most scores can be generalized to the multi-class setting. Often this is achieved by only considering "correct class" and "incorrect class" in order to make it a binary classification, then you average (weighted by the proportion of observation in the class) the score for each classes.

* **TP** / **TN** / **FN** / **FP:** Best understood through a $$2*2$$ [confusion matrix](#visual-metrics){:.mdLink}.

<div markdown="1">
![confusion matrix](/images/blog/glossary-old/confusion-matrix.png){:width='477px'}
</div>

* **Accuracy:** correctly classified fraction of observations. 
	*  $ Acc = \frac{Real Positives}{Total} = \frac{TP+FN}{TP+FN+TN+FP}$
	* :bulb: <span class="intuitionText"> In general, how much can we trust the predictions ? </span>
	* :wrench: <span class="practiceText"> Use if no class imbalance and cost of error is the same for both types  </span>
* **Precision** fraction of positive predictions that were actually positive. 
	* $ Prec = \frac{TP}{Predicted Positives} = \frac{TP}{TP+FP}$
	* :bulb: <span class="intuitionText"> How much can we trust positive predictions ? </span>
	* :wrench: <span class="practiceText"> Use if FP are the worst errors </span>
* **Recall** fraction of positive observations that have been correctly predicted. 
	* $ Rec = \frac{TP}{Actual Positives} = \frac{TP}{TP+FN}$
	* :bulb: <span class="intuitionText"> How many actual positives will we find? </span>
	* :wrench: <span class="practiceText"> Use if FN are the worst errors  </span>
* **F1-Score** harmonic mean (good for averaging rates) of recall and precision.
	* $F1 = 2 \frac{Precision * Recall}{Precision + Recall}$
    * If recall is $\beta$ time more important than precision use $F_{\beta} = (1+\beta^2) \frac{Precision * Recall}{\beta^2 Precision + Recall}$
	* :bulb: <span class="intuitionText"> How much to trust our algorithms for the positive class</span>
	* :wrench: <span class="practiceText"> Use if the positive class is the most important one (*i.e.* want a *detector* rather than a *classifier*)</span>

* **Specificity** recall for the negative negatives. 
    * $ Spec = \frac{TN}{Actual Negatives} = \frac{TN}{TN+FP}$
    
* **Log-Loss** measures performance when model outputs a probability $\hat{y_{ic}}$ that observation $n$ is in class $c$
	* Also called **Cross entropy loss** or **logistic loss**
	* $logLoss = - \frac{1}{N} \sum^N_{n=1} \sum^C_{c=1} y_{nc} \ln(\hat{y}_{nc})$
	* Use the natural logarithm for consistency
	* Incorporates the idea of probabilistic confidence
  * Log Loss is the metric that is minimized through [Logistic Regression](#logistic-regression){:.mdLink} and more generally [Softmax](#softmax){:.mdLink}
  * :bulb: <span class="intuitionText"> Penalizes more if the model is confident but wrong (see graph below)</span>
  * :bulb: <span class="intuitionText"> Log-loss is the</span>  [cross entropy](#cross-entropy){:.mdLink} <span class="intuitionText"> between the distribution of the true labels and the predictions</span> 
  * :wrench: <span class="practiceText"> Use when you are interested in outputting confidence of results </span>
  * The graph below shows the log loss depending on the confidence of the algorithm that an observation should be classed in the correct category. For multiple observation we compute the log loss of each and then average them.

  <div markdown="1">
  ![log loss](/images/blog/glossary-old/log-loss.png){:width='477px'}
  </div>

* **Cohen's Kappa** Improvement of your classifier compared to always guessing the most probable class
  * $\kappa = \frac{accuracy - percent_{MaxClass}}{1 - percent_{MaxClass}}$
  * Often used to computer inter-rater (*e.g.* 2 humans) reliability: $\kappa = \frac{p_o- p_e}{1 - p_e}$ where $p_o$ is the observed agreement and $p_e$ is the expected agreement due to chance.
  * $ \kappa \leq 1$ (if $<0$ then useless).
  * :bulb: <span class='intuitionText'>Accuracy improvement weighted by class imbalance </span> .
  * :wrench: <span class='practiceText'> Use when high class imbalance and all classes are of similar importance</span>

* **AUC** **A**rea **U**nder the **C**urve. Summarizes curves in a single metric.
  * It normally refers to the [ROC](#visual-metrics){:.mdLink} curve. Can also be used for other curves like the precision-recall one.
  * :bulb: <span class='intuitionText'> Probability that a randomly selected positive observation has is predicted with a higher score than a randomly selected negative observation </span> .
  * :mag: <span class='noteText'> AUC evaluates results at all possible cut-off points. It gives better insights about how well the classifier is able to separate between classes </span>. This makes it very different from the other metrics that typically depend on the cut-off threshold (*e.g.* 0.5 for [Logistic Regression](#logistic-regression){:.mdLink}).
  * :wrench: <span class='practiceText'> Use when building a classifier for users that will have different needs (they could tweak the cut-off point)</span> . From my experience AUC is widely used in statistics (~go-to metric in bio-statistics) but less in machine learning.
  * Random predictions: $AUC = 0.5$. Perfect predictions: $AUC=1$.
 
#### Visual Metrics
{:.no_toc}

* **ROC Curve** : **R**eceiver **O**perating **C**haracteristic
  * Plot showing the TP rate vs the FP rate, over a varying threshold.
  * This plot from [wikipedia](https://commons.wikimedia.org/wiki/File:ROC_curves.svg){:.mdLink} shows it well:
  
<div markdown="1">
![ROC curve](/images/blog/glossary-old/ROC.png){:width='477px'}
</div>

* **Confusion Matrix** a $C*C$ matrix which shows the number of observation of class $c$ that have been labeled $c', \ \forall c=1 \ldots C \text{ and } c'=1\ldots C$
    * :mag: <span class='noteText'> Be careful: People are not consistent with the axis :you can find real-predicted and predicted-real  </span> .
    * Best understood with an example:

    <div markdown="1">
    ![Multi Confusion Matrix](/images/blog/glossary-old/multi-confusion-matrix.png){:width='477px'}
    </div>

:information_source: <span class="resources"> Resources </span>: [Additional scores based on confusion matrix](https://en.wikipedia.org/wiki/Confusion_matrix){:.mdLink}

## Generative vs Discriminative 
These two major model types, distinguish themselves by the approach they are taking to learn. Although these distinctions are not task-specific task, you will most often hear those in the context of [classification](#classification){:.mdLink}.

### Differences
{:.no_toc}

In [classification](#classification){:.mdLink}, the task is to identify the category $y$ of an observation, given its features $\mathbf{x}$: $y \vert \mathbf{x}$. There are 2 possible approaches:

* **Discriminative** learn the *decision boundaries* between classes.
    * :bulb: <span class='intuitionText'> Tell me in which class is this observation given past data</span>. 
    * Can be **probabilistic** or **non-probabilistic** models. If probabilistic, the prediction is $\hat{y}=arg\max_{y=1 \ldots C} \, p(y \vert \mathbf{x})$. If non probabilistic, the model "draws" a boundary between classes, if the point $\mathbf{x}$ is on one side of of the boundary then predict $y=1$ if it is on the other then $y=2$ (multiple boundaries for multiple class).
    * Directly models what we care about: $y \vert \mathbf{x}$.
    * :school_satchel: As an example, for language classification, the  discriminative model would learn to <span class='exampleText'>distinguish between languages from their sound but wouldn't understand anything</span>.

* **Generative** model the *distribution* of each classes.
    * :bulb: <span class='intuitionText'> First "understand" the meaning of the data, then use your knowledge to classify</span>. 
    * Model the joint distribution $p(y,\mathbf{x})$ (often using $p(y,\mathbf{x})=p(\mathbf{x} \vert y)p(y)$). Then find the desired conditional probability through Bayes theorem: $p(y \vert \mathbf{x})=\frac{p(y,\mathbf{x})}{p(\mathbf{x})}$. Finally, predict $\hat{y}=arg\max_{y=1 \ldots C} \, p(y \vert \mathbf{x})$ (same as discriminative).
    * Generative models often use more assumptions to as t is a harder task.
    * :school_satchel: To continue with the previous example, the generative model would first <span class='exampleText'>learn how to speak the language and then classify from which language the words come from</span>.

### Pros / Cons
{:.no_toc}

Some of advantages / disadvantages are equivalent with different wording. These are rule of thumbs !

* **Discriminative**:
    <ul style="list-style: none;">
      <li > :white_check_mark: Such models <span class="advantageText">need less assumptions</span>  as they are tackling an easier problem. </li>
      <li > :white_check_mark: <span class="advantageText"> Often less bias => better if more data.</span> </li>
      <li > Often :x:<span class="disadvantageText"> slower convergence rate </span>. Logistic Regression requires $O(d)$ observations to converge to its asymptotic error. </li>
      <li > :x: <span class="disadvantageText"> Prone to over-fitting </span> when there's less data, as it doesn't make assumptions to constrain it from finding inexistent patterns.  </li>
      <li > Often :x: <span class="disadvantageText"> More variance. </span> </li>
      <li > :x: <span class="disadvantageText"> Hard to update the model </span> with new data (online learning). </li>
      <li > :x: <span class="disadvantageText"> Have to retrain model when adding new classes. </span> </li>
      <li > :x: <span class="disadvantageText"> In practice needs additional regularization / kernel / penalty functions.</span> </li>
    </ul >


* **Generative** 
  <ul style="list-style: none;">
      <li > :white_check_mark: <span class="advantageText"> Faster convergence rate => better if less data </span>. Naive Bayes only requires $O(\log(d))$ observations to converge to its asymptotic rate. </li>
      <li > Often :white_check_mark: <span class="advantageText"> less variance. </span> </li>
      <li > :white_check_mark: <span class="advantageText"> Can easily update the model  </span> with new data (online learning).  </li>
      <li > :white_check_mark: <span class="advantageText"> Can generate new data </span> by looking at $p(\mathbf{x} \vert y)$.  </li>
      <li > :white_check_mark: <span class="advantageText"> Can handle missing features</span> .  </li>
      <li > :white_check_mark: <span class="advantageText"> You don't need to retrain model when adding new classes </span>  as the parameters of classes are fitted independently.</li>
      <li > :white_check_mark: <span class="advantageText"> Easy to extend to the semi-supervised case. </span>  </li>
      <li > Often :x: <span class="disadvantageText"> more Biais. </span> </li>
      <li > :x: <span class="disadvantageText"> Uses computational power to compute something we didn't ask for.</span> </li>

    </ul >

:wrench: <span class='practice'> Rule of thumb </span>: If your need to train the best classifier on a large data set, use a **discriminative model**. If your task involves more constraints (online learning, semi supervised learning, small dataset, ...) use a **generative model**.

<div class="exampleBoxed" markdown="1">

Let's illustrate the advantages and disadvantage of both methods with an <span class='exampleText'> example </span> . Suppose we are asked to construct a classifier for the "true distribution" below. There are two training sets: "small sample" and "large sample". Suppose that the generator assumes point are generated from a Gaussian. 


<div class="col-xs-4" markdown="1">
![discriminative vs generative true distribution](/images/blog/glossary-old/discriminative-generative-true.png)
</div>

<div class="col-xs-4" markdown="1">
![discriminative vs generative small sample](/images/blog/glossary-old/discriminative-generative-small.png)
</div>

<div class="col-xs-4" markdown="1">
![discriminative vs generative large sample](/images/blog/glossary-old/discriminative-generative-large.png)
</div>

How well will the algorithms distinguish the classes in each case ?

* **Small Sample**:
    * The *discriminative* model never saw examples at the bottom of the blue ellipse. It will not find the correct decision boundary there.
    * The *generative* model assumes that the data follows a normal distribution (ellipse). It will therefore infer the correct decision boundary without ever having seen data points there!

<div class="col-xs-6" markdown="1">
![small sample discriminative](/images/blog/glossary-old/small-discriminative.png)
</div>

<div class="col-xs-6" markdown="1">
![small sample generative](/images/blog/glossary-old/small-generative.png)
</div>

.

* **Large Sample**:
    * The *discriminative* model is not restricted by assumptions and can find small red cluster inside the blue one.
    * The *generative* model assumes that the data follows a Gaussian distribution (ellipse) and won't be able to find the small red cluster.

<div class="col-xs-6" markdown="1">
![large sample discriminative](/images/blog/glossary-old/large-discriminative.png)
</div>

<div class="col-xs-6" markdown="1">
![large sample generative](/images/blog/glossary-old/large-generative.png)
</div>

This was simply an example that hopefully illustrates the advantages and disadvantages of needing more assumptions. Depending on their assumptions, some generative models would find the small red cluster.
</div>

### Examples of Algorithms
{:.no_toc}

#### Discriminative
* [Logistic Regression](#logistic-regression){:.mdLink}
* [Softmax](#softmax){:.mdLink}
* Traditional Neural Networks
* Conditional Random Fields
* Maximum Entropy Markov Model
* [Decision Trees](#decision-trees){:.mdLink}


#### Generative
* [Naives Bayes](#naive-bayes){:.mdLink}
* Gaussian Discriminant Analysis
* Latent Dirichlet Allocation
* Restricted Boltzmann Machines
* Gaussian Mixture Models
* Hidden Markov Models 
* Sigmoid Belief Networks
* Bayesian networks
* Markov random fields

#### Hybrid
* Generative Adversarial Networks

:information_source: <span class='resources'> Resources </span> : A. Ng and M. Jordan have a [must read paper](https://ai.stanford.edu/~ang/papers/nips01-discriminativegenerative.pdf){:.mdLink} on the subject, T. Mitchell summarizes very well these concepts in [his slides](http://www.cs.cmu.edu/~ninamf/courses/601sp15/slides/07_GenDiscr2_2-4-2015.pdf){:.mdLink}, and section 8.6 of [K. Murphy's book](https://www.cs.ubc.ca/~murphyk/MLbook/){:.mdLink} has a great overview of pros and cons, which strongly influenced the devoted section above.

## Information Theory

### Information Content

Given a random variable $X$ and a possible outcome $x_i$ associated with a probability $p_X(x_i)=p_i$, the information content (also called self-information or surprisal) is defined as :

$$\operatorname{I} (p_i) = - log(p_i)$$

:bulb: <span class="intuition"> Intuition </span>: The "information" of an event is higher when the event is less probable. Indeed, if an event is not surprising, then learning about it does not convey much additional information.

:school_satchel: <span class='example'> Example </span> : "it is currently cold in Antarctica" does not convey much information as you knew it with high probability. "It is currently very warm in Antarctica" carries a lot more information and you would be surprised about it.

:mag: <span class="note"> Side note </span> : 

* $\operatorname{I} (p_i) \in [0,\infty[$
* Don't confuse the *information content* in information theory with the everyday word which refers to "meaningful information". <span class="exampleText"> A book with random letters will have more information content because each new letter would be a surprise to you. But it will definitely not have more meaning than a book with English words </span>.

### Entropy

<details open>
  <summary>Long Story Short</summary>
  <div markdown="1">
$$H(X) = H(p) \equiv \mathbb{E}\left[\operatorname{I} (p_i)\right] = \sum_{i=1}^K p_i \ \log(\frac{1}{p_i}) = - \sum_{i=1}^K p_i\  log(p_i)$$

:bulb: <span class="intuition"> Intuition </span>:

* The entropy of a random variable is the expected [information-content](#entropy){:.mdLink}. I.e. the <span class="intuitionText"> expected amount of surprise you would have by observing a random variable  </span>. 
* <span class="intuitionText"> Entropy is the expected number of bits (assuming $log_2$) used to encode an observation from a (discrete) random variable under the optimal coding scheme </span>. 


:mag: <span class="note"> Side notes </span> :

* $H(X) \geq 0$
* Entropy is maximized when all events occur with uniform probability. If $X$ can take $n$ values then : $max(H) = H(X_{uniform})= \sum_i^K \frac{1}{K} \log(\frac{1}{ 1/K} ) = \log(K)$

</div>
</details>

<p></p>


<details>
  <summary>Long Story Long</summary>
  <div markdown="1">
  
The concept of entropy is central in both thermodynamics and information theory, and I find that quite amazing. It originally comes from statistical thermodynamics and is so important, that it is carved on Ludwig Boltzmann's grave (one of the father of this field). You will often hear:

* **Thermodynamics**: *Entropy is a measure of disorder*
* **Information Theory**: *Entropy is a measure of information*

These 2 way of thinking may seem different but in reality they are exactly the same. They essentially answer: <span class="intuitionText"> how hard is it to describe this thing? </span>

I will focus here on the information theory point of view, because its interpretation is more intuitive for machine learning. I also don't want to spend to much time thinking about thermodynamics, as [people that do often commit suicide](http://www.eoht.info/page/Founders+of+thermodynamics+and+suicide){:.mdLink} :flushed:.

$$H(X) = H(p) \equiv \mathbb{E}\left[\operatorname{I} (p_i)\right] = \sum_{i=1}^K p_i \ \log(\frac{1}{p_i}) = - \sum_{i=1}^K p_i\  log(p_i)$$

 In information theory there are 2 intuitive way of thinking of entropy. These are best explained through an <span class="example"> example </span> : 

<div class="exampleBoxed">
<div markdown="1">
:school_satchel: Suppose my friend [Claude](https://en.wikipedia.org/wiki/Claude_Shannon){:.mdLink} offers me to join him for a NBA game (Cavaliers vs Spurs) tonight. Unfortunately I can't come, but I ask him to record who scored each field goals. Claude is very geeky and uses a binary phone which can only write 0 and 1. As he doesn't have much memory left, he wants to use the smallest possible number of bits.

1. From previous games, Claude knows that Lebron James will very likely score more than the old (but awesome :basketball: ) Manu Ginobili. Will he use the same number of bits to indicate that Lebron scored, than he will for Ginobili ? Of course not, he will allocate less bits for Lebron's buckets as he will be writing them down more often. He's essentially exploiting his knowledge about the distribution of field goals to reduce the expected number of bits to write down. It turns out that if he knew the probability $p_i$ of each player $i$ to score, he should encode their name with $nBit(p_i)=\log_2(1/p_i)$ bits. This has been intuitively constructed by Claude (Shannon) himself as it is the only measure (up to a constant) that satisfies axioms of information measure. The intuition behind this is the following:
	*  <span class="intuitionText"> Multiplying probabilities of 2 players scoring should result in adding their bits. </span> Indeed imagine Lebron and Ginobili have respectively 0.25 and 0.0625 probability of scoring the next field goal. Then, the probability that Lebron scores the 2 next field goals would be the same than Ginobili scoring a single one ($p(Lebron)*p(Lebron) = 0.25 * 0.25 = 0.0625 = Ginobili$). We should thus allocate 2 times less bits for Lebron, so that on average we always add the same number of bits per observation. $nBit(Lebron) = \frac{1}{2} * nBit(Ginobili) = \frac{1}{2} * nBit(p(Lebron)^2)$. The logarithm is a function that turns multiplication into sums as required. The number of bits should thus be of the form $nBit(p_i) = \alpha * \log(p_i) + \beta $
	* <span class="intuitionText"> Players that have higher probability of scoring should be encoded by a lower number of bits </span>. I.e $nBit$ should decrease when $p_i$ increases: $nBit(p_i) = - \alpha * \log(p_i) + \beta, \alpha > 0  $
	* <span class="intuitionText"> If Lebron had $100%$ probability of scoring, why would I have bothered asking Claude to write anything down ? I would have known everything *a priori* </span>. I.e $nBit$ should be $0$ for $p_i = 1$ : $nBit(p_i) = \alpha * \log(p_i), \alpha > 0  $

2. Now Claude sends me the message containing information about who scored each bucket. Seeing that Lebron scored will surprise me less than Ginobili. I.e Claude's message gives me more information when telling me that Ginobili scored. If I wanted to quantify my surprise for each field goal, I should make a measure that satisfies the following conditions:
	* <span class="intuitionText">The lower the probability of a player to score, the more surprised I will be </span>. The measure of surprise should thus be a decreasing function of probability: $surprise(x_i) = -f(p_i) * \alpha, \alpha > 0$.
	* Supposing that players scoring are independent of one another, it's reasonable to ask that my surprise seeing Lebron and Ginobili scoring in a row should be the same than the sum of my surprise seeing that Lebron scored and my surprise seeing that Ginobili scored. *I.e.* <span class="intuitionText"> Multiplying independent probabilities should sum the surprise </span>: $surprise(p_i * x_j) = surprise(p_i) + surprise(p_j)$.
	* Finally, <span class="intuitionText"> the measure should be continuous given probabilities </span>. $surprise(p_i) = -\log(p_{i}) * \alpha, \alpha > 0$

Taking $\alpha = 1 $ for simplicity, we get $surprise(p_i) = -log(p_i) =  nBit(p_i)$. We thus derived a formula for computing the surprise associated with event $x_i$ and the optimal number of bits that should be used to encode that event. This value is called information content $I(p_i)$. <span class="intuitionText">In order to get the average surprise / number of bits associated with a random variable $X$ we simply have to take the expectation over all possible events</span> (i.e average weighted by probability of event). This gives us the entropy formula $H(X) = \sum_i p_i \ \log(\frac{1}{p_i}) = - \sum_i p_i\  log(p_i)$

</div>
</div>

From the example above we see that entropy corresponds to : 
<div class="intuitionText">
<div markdown="1">
* **to the expected number of bits to optimally encode a message**
* **the average amount of information gained by observing a random variable** 
</div>
</div>

:mag: <span class="note"> Side notes </span> :

* From our derivation we see that the function is defined up to a constant term $\alpha$. This is the reason why the formula works equally well for any logarithmic base, indeed changing the base is the same as multiplying by a constant. In the context of information theory we use $\log_2$.
* Entropy is the reason (second law of thermodynamics) why putting an ice cube in your *Moscow Mule* (my go-to drink) doesn't normally make your ice cube colder and your cocktail warmer. I say "normally" because it is possible but very improbable : ponder about this next time your sipping your own go-to drink :smirk: ! 

:information_source: <span class="resources"> Resources </span>: Excellent explanation of the link between [entropy in thermodynamics and information theory](http://www.askamathematician.com/2010/01/q-whats-the-relationship-between-entropy-in-the-information-theory-sense-and-the-thermodynamics-sense/){:.mdLink}, friendly [ introduction to entropy related concepts](https://rdipietro.github.io/friendly-intro-to-cross-entropy-loss/){:.mdLink}

</div>
</details>
<p></p>

### Differential Entropy
Differential entropy (= continuous entropy), is the generalization of entropy for continuous random variables.

Given a continuous random variable $X$ with a probability density function $f(x)$:

$$h(X) = h(f) := - \int_{-\infty}^{\infty} f(x) \log {f(x)} \ dx$$

If you had to make a guess, which distribution maximizes entropy for a given variance ? You guessed it : it's the **Gaussian distribution**.

:mag: <span class="note"> Side notes </span> : Differential entropy can be negative.

### Cross Entropy
We [saw that](#entropy){:.mdLink} entropy is the expected number of bits used to encode an observation of $X$ under the optimal coding scheme. In contrast <span class="intuitionText"> cross entropy is the expected number of bits to encode an observation of $X$ under the wrong coding scheme</span>. Let's call $q$ the wrong probability distribution that is used to make a coding scheme. Then we will use $-log(q_i)$ bits to encode the $i^{th}$ possible values of $X$. Although we are using $q$ as a wrong probability distribution, the observations will still be distributed based on $p$. We thus have to take the expected value over $p$ :

$$H(p,q) = \mathbb{E}_p\left[\operatorname{I} (q_i)\right] = - \sum_i p_i \log(q_i)$$

From this interpretation it naturally follows that:
* $H(p,q) > H(p), \forall q \neq p$
* $H(p,p) = H(p)$

:mag: <span class="note"> Side notes </span> : Log loss is often called cross entropy loss, indeed it is the cross-entropy between the distribution of the true labels and the predictions.

### Kullback-Leibler Divergence
The Kullback-Leibler Divergence (= relative entropy = information gain) from $q$ to $p$ is simply the difference between the cross entropy and the entropy:

$$
\begin{align*} 
D_{KL}(p\|q) &= H(p,q) - H(p) \\
&= [- \sum_i p_i \log(q_i)] - [- \sum_i p_i \log(p_i)] \\
&= \sum_i p_i \log(\frac{p_i}{q_i})
\end{align*} 
$$

:bulb: <span class="intuition"> Intuition </span>

* KL divergence corresponds to the number of additional bits you will have to use when using an encoding scheme based on the wrong probability distribution $q$ compared to the real $p$ .
* KL divergence says in average how much more surprised you will be by rolling a loaded dice but thinking it's fair, compared to the surprise of knowing that it's loaded.
* KL divergence is often called the **information gain** achieved by using $p$ instead of $q$
* KL divergence can be thought as the "distance" between 2 probability distribution. Mathematically it's not a distance as it's none symmetrical. It is thus more correct to say that it is a measure of how a probability distribution $q$ diverges from an other one $p$.
	
KL divergence is often used with probability distribution of continuous random variables. In this case the expectation involves integrals:

$$D_{KL}(p \parallel q) = \int_{- \infty}^{\infty} p(x) \log(\frac{p(x)}{q(x)}) dx$$

In order to understand why KL divergence is not symmetrical, it is useful to think of a simple example of a dice and a coin (let's indicate head and tails by 0 and 1 respectively). Both are fair and thus their PDF is uniform. Their entropy is trivially: $H(p_{coin})=log(2)$ and $H(p_{dice})=log(6)$. Let's first consider $D_{KL}(p_{coin} \parallel p_{dice})$. The 2 possible events of $X_{dice}$ are 0,1 which are also possible for the coin. The average number of bits to encode a coin observation under the dice encoding, will thus simply be $log(6)$, and the KL divergence is of $log(6)-log(2)$ additional bits. Now let's consider the problem the other way around: $D_{KL}(p_{dice} \parallel p_{coin})$. We will use $log(2)=1$ bit to encode the events of 0 and 1. But how many bits will we use to encode $3,4,5,6$ ? Well the optimal encoding for the dice doesn't have any encoding for these as they will never happen in his world. The KL divergence is thus not defined (division by 0). The KL divergence is thus not symmetric and cannot be a distance.

:mag: <span class="note"> Side notes </span> : Minimizing cross entropy with respect to $q$ is the same as minimizing $D_{KL}(p \parallel q)$. Indeed the 2 equations are equivalent up to an additive constant (the entropy of $p$) which doesn't depend on $q$.

### Mutual Information

$$
\begin{align*} 
\operatorname{I} (X;Y) = \operatorname{I} (Y;X) 
&:= D_\text{KL}\left(p(x, y) \parallel p(x)p(y)\right) \\
&=  \sum_{y \in \mathcal Y} \sum_{x \in \mathcal X}
    { p(x,y) \log{ \left(\frac{p(x,y)}{p(x)\,p(y)} \right) }}
\end{align*} 
$$

:bulb: <span class="intuition"> Intuition </span>: The mutual information between 2 random variables X and Y measures how much (on average) information about one of the r.v. you receive by knowing the value of the other. If $X,\ Y$ are independent, then knowing $X$ doesn't give information about $Y$ so $\operatorname{I} (X;Y)=0$ because $p(x,y)=p(x)p(y)$. The maximum information you can get about $Y$ from $X$ is all the information of $Y$ *i.e.* $H(Y)$. This is the case for $X=Y$ : $\operatorname{I} (Y;Y)= \sum_{y \in \mathcal Y} p(y) \log{ \left(\frac{p(y)}{p(y)\,p(y)} \right) = H(Y) }$ 

:mag: <span class="note"> Side note </span> : 

* The mutual information is more similar to the concept of [entropy](#entropy){:.mdLink} than to [information content](#information-content){:.mdLink}. Indeed, the latter was only defined for an *outcome* of a random variable, while the entropy and mutual information are defined for a r.v. by taking an expectation.
* $\operatorname{I} (X;Y) \in [0, min(\operatorname{I} (X), \operatorname{I} (Y;Y))]$
* $\operatorname{I} (X;X) =  \operatorname{I} (X)$
* $\operatorname{I} (X;Y) =  0 \iff X \,\bot\, Y$
* The generalization of mutual information to $V$ random variables $X_1,X_2,\ldots,X_V$ is the [Total Correlation](https://en.wikipedia.org/wiki/Total_correlation){:.mdLink}: $C(X_1, X_2, \ldots, X_V) := \operatorname{D_{KL}}\left[ p(X_1, \dots, X_V) \parallel p(X_1)p(X_2)\dots p(X_V)\right]$. It denotes the total amount of information shared across the entire set of random variables. The minimum $C_\min=0$ when no r.v. are statistically dependent. The maximum total correlation occurs when a single r.v. determines all the others : $C_\max = \sum_{i=1}^V H(X_i)-\max\limits_{X_i}H(X_i)$.
* Data Processing Inequality: for any Markov chain $X \rightarrow Y \rightarrow Z$: $\operatorname{I} (X;Y) \geq \operatorname{I} (X;Z)$
* Reparametrization Invariance: for invertible functions $\phi,\psi$: $\operatorname{I} (X;Y) = \operatorname{I} (\phi(X);\psi(Y))$

### Machine Learning and Entropy
This is all interesting, but why are we talking about information theory concepts in machine learning :sweat_smile: ? Well it turns our that many ML algorithms can be interpreted with entropy related concepts.

The 3 major ways we see entropy in machine learning are through:

* **Maximizing information gain** (i.e entropy) at each step of our algorithm. <span class="exampleText">Example</span>:
	
	* When building <span class="exampleText">decision trees you greedily select to split on the attribute which maximizes information gain</span> (i.e the difference of entropy before and after the split). Intuitively you want to know the value of the attribute, that decreases the randomness in your data by the largest amount.

* **Minimizing KL divergence between the actual unknown probability distribution of observations $p$ and the predicted one $q$**. <span class="exampleText">Example</span>:

	* The Maximum Likelihood Estimator (MLE) of our parameters $\hat{ \theta }_{MLE}$ <span class="exampleText"> are also the parameter which minimizes the KL divergence between our predicted distribution $q_\theta$ and the actual unknown one $p$ </span> (or the cross entropy). I.e 

$$\hat{ \theta }_{MLE} = arg\min_{ \theta } \, NLL= arg\min_{ \theta } \, D_{KL}(p \parallel q_\theta ) = arg\min_{ \theta } \, H(p,q_\theta ) $$

* **Minimizing  KL divergence between the computationally intractable $p$ and a simpler approximation $q$**. Indeed machine learning is not only about theory but also about how to make something work in practice.<span class="exampleText">Example</span>:

  - This is the whole point of <span class="exampleText"> **Variational Inference** (= variational Bayes) which approximates posterior probabilities of unobserved variables that are often intractable due to the integral in the denominator. Thus turning the inference problem to an optimization one</span>. These methods are an alternative to Monte Carlo sampling methods for inference (*e.g.* Gibbs Sampling). In general sampling methods are slower but asymptotically exact.


## No Free Lunch Theorem

*There is no one model that works best for every problem.*

Let's try predicting the next fruit in the sequence: 

<div class="centerContainer">
:tangerine: :apple: :tangerine: :apple: :tangerine: ...
</div>

You would probably say :apple: right ? Maybe with a lower probability you would say :tangerine: . But have you thought of saying :watermelon: ? I doubt it. I never told you that the sequence was constrained in the type of fruit, but naturally we make assumptions that the data "behaves well". <span class='intuitionText'> The point here is that without any knowledge/assumptions on the data, all future data are equally probable. </span> 

The theorem builds on this, and states that every algorithm has the same performance when averaged over all data distributions. So the average performance of a deep learning classifier is the same as random classifiers.

:mag: <span class='note'> Side Notes </span> :
* You will often hear the name of this theorem when someone asks a question starting with "what is the **best** [...] ?".
* In the real world, things tend to "behave well". They are for example often (locally) continuous. In such settings some algorithms are definitely better than others.
* Since the theorem publication in 1996, other methods have kept the lunch metaphor. For example: [kitchen sink](https://en.wikipedia.org/wiki/Kitchen_sink_regression){:.mdLink} algorithms, [random kitchen sink](https://people.eecs.berkeley.edu/~brecht/papers/08.rah.rec.nips.pdf){:.mdLink}, [fastfood](https://arxiv.org/pdf/1408.3060.pdf){:.mdLink}, [à la carte](https://pdfs.semanticscholar.org/7e66/9999c097479c35e3f31aabdd2888f74b2e3e.pdf){:.mdLink}, and that's one of the reason why I decided to stick with fruit examples in this blog :wink:.
* The theorem has been extend to optimization and search algorithms.

:information_source: <span class='resources'> Resources </span> : D. Wolpert's [proof](http://citeseerx.ist.psu.edu/viewdoc/download?doi=10.1.1.390.9412&rep=rep1&type=pdf){:.mdLink}.


## Parametric vs Non Parametric
These 2 types of methods distinguish themselves based on their answer to the following question: "Will I use the same amount of memory to store the model trained on $100$ examples than to store a model trained on $10 000$ of them ? "
If yes then you are using a *parametric model*. If not, you are using a *non-parametric model*.

+ **Parametric**:
  - :bulb: <span class='intuitionText'> The memory used to store a model trained on $100$ observations is the same as for a model trained on $10 000$ of them  </span>. 
  - I.e: The number of parameters is fixed.
  - :white_check_mark: <span class='advantageText'> Computationally less expensive </span> to store and predict.
  - :white_check_mark: <span class='advantageText'> Less variance. </span> 
  - :x: <span class='disadvantageText'> More bias.</span> 
  - :x: <span class='disadvantageText'> Makes more assumption on the data</span> to fit less parameters.
  - :school_satchel: <span class='example'> Example </span> : [K-Means](#k-means){:.mdLink} clustering, [Linear Regression](#linear-regression){:.mdLink}, Neural Networks:
  
  <div markdown="1">
  ![Linear Regression](/images/blog/glossary-old/Linear-regression.png){:width='300px'}
  </div>


+ **Non Parametric**: 
  - :bulb: <span class='intuitionText'> I will use less memory to store a model trained on $100$ observation than for a model trained on $10 000$ of them  </span>. 
  - I.e: The number of parameters is grows with the training set.
  - :white_check_mark: <span class='advantageText'> More flexible / general.</span> 
  - :white_check_mark: <span class='advantageText'> Makes less assumptions. </span> 
  - :white_check_mark: <span class='advantageText'> Less bias. </span> 
  - :x: <span class='disadvantageText'> More variance.</span> 
  - :x: <span class='disadvantageText'> Bad if test set is relatively different than train set.</span> 
  - :x: <span class='disadvantageText'> Computationally more expensive </span> as it has to store and compute over a higher number of "parameters" (unbounded).
  - :school_satchel: <span class='example'> Example </span> : [K-Nearest Neighbors](#k-nearest-neighbors){:.mdLink} clustering, RBF Regression, Gaussian Processes:

  <div markdown="1">
  ![RBF Regression](/images/blog/glossary-old/RBF-regression.png){:width='300px'}
  </div>

:wrench: <span class='practice'> Practical </span> : <span class='practiceText'>Start with a parametric model</span>. It's often worth trying a non-parametric model if: you are doing <span class='practiceText'>clustering</span>, or the training data is <span class='practiceText'>not too big but the problem is very hard</span>.

:mag: <span class='note'> Side Note </span> : Strictly speaking any non-parametric model could be seen as a infinite-parametric model. So if you want to be picky: next time you hear a colleague talking about non-parametric models, tell him it's in fact parametric. I decline any liability for the consequence on your relationship with him/her :sweat_smile: . 

