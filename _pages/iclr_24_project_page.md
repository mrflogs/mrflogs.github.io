---
layout: project_page
permalink: /ICLR24
title: A Hard-to-Beat Baseline for Training-free CLIP-based Adaptation
authors: 
    <a href="https://zhengbo.wang/"> Zhengbo Wang<sup>1,2</sup></a> &nbsp;  
    <a href="https://liangjian.xyz/">Jian Liang<sup>2,3&dagger;</sup></a> &nbsp;  
    <a href="https://tomsheng21.github.io/">Lijun Sheng<sup>1,2</sup></a> &nbsp;  
    <a href="https://rhe-web.github.io/">Ran He<sup>2,3</sup></a> &nbsp;  
    <a href="http://staff.ustc.edu.cn/~zlwang/index_en.html">Zilei Wang<sup>1</sup></a> &nbsp;
    <a href="http://www.cbsr.ia.ac.cn/users/tnt/tnt.htm">Tieniu Tan<sup>2,4</sup></a>
affiliations:
    <sup>1</sup> University of Science and Technology of China <br>
    <sup>2</sup> CRIPAC & MAIS, Institute of Automation, Chinese Academy of Sciences <br>
    <sup>3</sup> School of Artificial Intelligence, University of Chinese Academy of Sciences <br>
    <sup>4</sup> Nanjing University <br>
paper: https://openreview.net/forum?id=Js5PJPHDyY
code: https://github.com/mrflogs/ICLR24
---

<script src="https://polyfill.io/v3/polyfill.min.js?features=es6"></script>
<script id="MathJax-script" async src="https://cdn.jsdelivr.net/npm/mathjax@3/es5/tex-mml-chtml.js"></script>
<script src="https://polyfill.io/v3/polyfill.min.js?features=es6"></script>
<script id="MathJax-script" async src="https://cdn.jsdelivr.net/npm/mathjax@3/es5/tex-mml-chtml.js"></script>

<!-- <div class="container is-max-desktop"> -->
<div class="columns is-centered has-text-centered">
  <div class="column is-four-fifths">
    <h2 class="title is-3">Abstract</h2>
    <div class="content has-text-justified">
Contrastive Language-Image Pretraining (CLIP) has gained popularity for its remarkable zero-shot capacity. Recent research has focused on developing efficient fine-tuning methods, such as prompt learning and adapter, to enhance CLIP's performance in downstream tasks. However, these methods still require additional training time and computational resources, which is undesirable for devices with limited resources. In this paper, we revisit a classical algorithm, Gaussian Discriminant Analysis (GDA), and apply it to the downstream classification of CLIP. Typically, GDA assumes that features of each class follow Gaussian distributions with identical covariance. By leveraging Bayes' formula, the classifier can be expressed in terms of the class means and covariance, which can be estimated from the data without the need for training. To integrate knowledge from both visual and textual modalities, we ensemble it with the original zero-shot classifier within CLIP. Extensive results on 17 datasets validate that our method surpasses or achieves comparable results with state-of-the-art methods on few-shot classification, imbalanced learning, and out-of-distribution generalization. In addition, we extend our method to base-to-new generalization and unsupervised learning, once again demonstrating its superiority over competing approaches. Our code is publicly available at <a href="https://github.com/mrflogs/ICLR24">https://github.com/mrflogs/ICLR24</a>.
        </div>
    </div>
</div>

<div class="columns is-centered">
  <div class="column is-four-fifths">
    <h2 class="title is-3">Method Overview</h2>
      <div style="text-align: center;">
        <img src="../assets/img/project_page_images/hard_to_beat/architecture.png" class="img-responsive" alt="overview" width="100%" style="margin:auto;padding-top: 1em"/>
      </div>
      <figcaption> <p class="text-justify">
          <span style="font-weight: bold; font-size: larger;">Figure 1. </span>
          <strong>The overview of our training-free method.</strong>
          In our method, we begin by extracting visual features from the training dataset using the CLIP visual encoder. 
          Next, we compute the mean vectors for each class and the shared precision matrix (inverse covariance) using Eq.(\ref{eq:precisionmatrix}).
          Through the Gaussian Discriminate Analysis (GDA), the weight and bias of the classifier can be expressed in terms of the mean vectors and the precision matrix, which can be derived from Eq.(\ref{eq:solution}) 
          <span style="color: red;">(the red formula in the figure.) </span>
          Finally, we enhance our method by ensembling the GDA classifier and the CLIP's zero-shot classifier, integrating the knowledge from visual and textual modalities.
      </p>
      </figcaption>
      <h3 class="title is-4">TL;DR</h3>
      <div class="content has-text-justified">
        <p>
        In this paper, we've revisited Gaussian Discriminate Analysis (GDA) within CLIP and found that it stands out as a hard-to-beat training-free baseline for CLIP-based adaptation, which surpasses previous state-of-the-art training-based and traing-free fine-tuning methods, thanks to its covariance assumption.
        Moreover,  we have expanded its application to include CLIP long-tail classification, base-to-new generalization, and unsupervised learning, showcasing its broad effectiveness.
        </p>
      </div>
      <h3 class="title is-4">Basic Setting</h3>
      <div>
        <p>
        Gaussian Dicriminate Analysis (GDA) is a well-known probability discriminative method in machine learning. 
        Without explicit learning such as gradient descent, GDA obtains the classifier by estimating the mean vectors and the covariance matrix of the data. 
        By utilizing the Bayes Formula, we can obtain the classification weights as follows:
        </p>
      </div>
      <div>
      \[
      \begin{equation}
        \label{eq:solution}
        w_i = \Sigma^{-1}\mu_i, \quad
        b_i = \log p_i - \frac{1}{2}\mu_i^T\Sigma^{-1}\mu_i, \tag{1}
      \end{equation}
      \]
      </div>
      <div>
      \[
      \begin{equation}
      \label{eq:precisionmatrix}
        \widehat{\Sigma^{-1}} = D((N-1)\hat{\Sigma} + tr(\hat{\Sigma})I_D)^{-1}, \tag{2}
      \end{equation}
      \]
      </div>
      <div>
      <p>
      We can further ensemble GDA with the CLIP zero-shot classifier.
      </p>
      </div>
      <div>
      \[
      \begin{equation}
        \label{eq:ensemble}
        logits = x_{test}W_c^T + \alpha (x_{test}W^T + b), \tag{3}
      \end{equation}
      \]
      </div>
      <div>
      <p>
      We apply this formula in CLIP few-shot classification and long-tailed classification. 
      </p>
      </div>
      <h3 class="title is-4">Extention to more scenarios</h3>
      <h4 class="title is-5">Base-to-New Generalization</h4>
      <div>
      For CLIP base-to-new generalization, the model is trained on the base dataset and tested on a new dataset with unseen classes.
      However, our method cannot be directly implemented in the scenario where data for the new classes is unavailable.
      Based on the observation that similar samples have similar statistical information, we propose that our method can be extended to new classes using the KNN algorithm.
      To achieve this, we utilize text embeddings of the new classes to query the training set and select the k nearest neighbors as the synthesized labeled data. 
The process is defined as follows:
      </div>
      <div>
      \[  
      \begin{equation}
        \tilde{\mathcal{D}}_{new} = \bigcup_{i=K+1}^M\{(x, i)|x\in NN^k(t_i, \mathcal{D})\}, \tag{4}
      \end{equation}
      \]
      </div>
      <h4 class="title is-5">Unsupervised Learning</h4>
      <p>
      In the unsupervised learning scenario, we only have the unlabeled data \(\{x_i\}_{i=1}^N\).
      Based on the Gaussian assumption in GDA, the unsupervised data \(\{x_i\}_{i=1}^N\) follow Gaussian mixture distribution.
      In order to maintain the simplicity of our method, we directly employ the EM algorithm for estimating the means and covariance matrix.
      To begin, we initialize the mean vectors and covariance using the zero-shot classifier, assuming equal priors for each Gaussian distribution.
      In the E-step, we calculate the probability of the unlabeled data \(\{x_i\}_{i=1}^N\) as follows:
      </p>
      <div>
      \[
      \begin{equation}
        \gamma_{ik} = \frac{\exp(f_k(x))}{\Sigma_{j=1}^K\exp{(f_j(x_i))}}, \tag{5}
      \end{equation}
      \]
      </div>
      <p>
      for the unlabeled data \(\{x_i\}_{i=1}^N\), and \(f\) is the logit function using Eq.(\ref{eq:ensemble}).
      Moving on to the M-step, we update the mean vectors and covariance matrix using the following formulas:
      </p>
      <div>
      \[
      \begin{equation}
        \mu_k = \frac{\sum_{i=1}^N\gamma_{ik}x_i}{\sum_{i=1}^N\gamma_{ik}}, \quad
        \Sigma =\frac{1}{K}\sum_{k=1}^K\frac{\sum_{i=1}^N\gamma_{ik}(x_i - \mu_k)(x_i - \mu_k)^T}{\sum_{i=1}^N\gamma_{ik}}. \tag{6}
      \end{equation}
      \]
      </div>
      <div>
      <p>
      Subsequently, we update the classifier using Eq.(\ref{eq:solution}) and repeat the EM process until convergence.
      </p>
      </div>
    <h2 class="title is-3">Experiments</h2>
    <h3 class="title is-4">Few-shot Classification</h3>
    <div style="text-align: center;">
      <img src="../assets/img/project_page_images/hard_to_beat/results_on_few_shot_classification.png" class="img-responsive" alt="overview" width="100%" style="margin:auto;padding-top: 1em"/>
    </div>
    <div style="text-align: center;">
      <img src="../assets/img/project_page_images/hard_to_beat/results_on_16_shot_dataset.png" class="img-responsive" alt="overview" width="100%" style="margin:auto;padding-top: 1em"/>
    </div>
    <h3 class="title is-4">Base-to-New Generalization</h3>
    <div style="text-align: center;">
      <img src="../assets/img/project_page_images/hard_to_beat/base_to_new.png" class="img-responsive" alt="overview" width="100%" style="margin:auto;padding-top: 1em"/>
    </div>
    <h3 class="title is-4">Long-tailed Classification</h3>
    <div style="text-align: center;">
      <img src="../assets/img/project_page_images/hard_to_beat/long_tailed.png" class="img-responsive" alt="overview" width="100%" style="margin:auto;padding-top: 1em"/>
    </div>
    <h3 class="title is-4">Unsupervised Learning</h3>
    <div style="text-align: center;">
      <img src="../assets/img/project_page_images/hard_to_beat/unsupervised_learning.png" class="img-responsive" alt="overview" width="100%" style="margin:auto;padding-top: 1em"/>
    </div>
  </div>
</div>




<section class="section" id="BibTeX">
  <div class="container is-max-desktop content">
    <h2 class="title">BibTeX</h2>
    <pre><code>@article{wang2024baseline,
  author    = {Wang, Zhengbo and Liang, Jian and Sheng, Lijun and He, Ran and Wang, Zilei and Tan, Tieniu},
  title     = {A Hard-to-Beat Baseline for Training-free CLIP-based Adaptation},
  journal   = {The Twelfth International Conference on Learning Representations (ICLR)},
  year      = {2024},
}</code></pre>
  </div>
</section>
