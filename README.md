# Linguistic Style Transfer 
Implementation of the paper `Disentangled Representation Learning for Non-Parallel Text Style Transfer`[(link)](https://www.aclweb.org/anthology/P19-1041.pdf) in Pytorch

## Abstract
  This paper tackles the problem of disentangling the latent representations of style and content in language models.
  We propose a simple yet effective approach, which incorporates auxiliary multi-task and adversarial objectives, for 
  style prediction and bag-of-words prediction, respectively. We show, both qualitatively and quantitatively, that the 
  style and content are indeed disentangled in the latent space. This disentangled latent representation learning can be                  applied to style transfer on non-parallel corpora. We achieve high performance in terms of transfer accuracy, content     preservation, and language fluency, in comparision to various previous approaches.

## Overview
 * Map the sentences to a latent space using VAE framework.
 * The latent space is artificially divided into style space and content space, and the model is encouraged to disentangle
    the latent space with respect to the above two features,namely, style and content.
 * To accomplish this, the VAE loss (ELBO) is augmented with two auxiliary losses,namely, multitask loss and adversary loss.
 * Multitask loss:
    * It operates on the latent space to ensure that it does contain the information we wish to encode,    i.e. it encourages    the style space and content space to preserve the style and content information respectively.
    * The style style classifier is trained to predict the style label label .
    * The content classifier is trained to predict the Bag of Words (BoW) representation of the sentence.
  * Adversarial Loss:
    * The adversarial loss, on the contrary, minimizes the predictability of information that should not be contained
      in a given latent space.
    * The disentanglement of style space and content space is accomplished by adversarial learning procedure.
    * Adversarial procedure is similar to that of the original GAN, where discriminator is trained to correctly classify 
      the samples and the generator is trained to fool the discriminator by producing samples indistinguishable from 
      the original data samples.
    * In this setting, for style space, the style discriminator is trained to predict the style label and the style generator
      is trained to increase the entropy of the predictions/softmax output since higher entropy corresponds to lesser
      information. Similarly, the same procedure is repeated for content space.
   * To address the posterior collapse issue that usually occurs when powerful decoders like LSTMs are used, sigmoid KL 
     annealing is used during training. Also, the latent embedding is concatenated to word embeddings at every time step of
     the decoder.
   * During the last epoch of training, average of style embeddings over whole train data is calculated for both the styles(positive and negative sentiment). These average positive and negative style embeddings are approximated to be 
     estimates of positive and negative style embeddings.
   * During inference phase, deteremine the content embedding of the sentence and concatenate the estimated style embedding
     of the opposite sentiment to it. Use this latent embedding for decoding/transfering style.
     
 ## Training and Inference
  Illustration of training and inference.    
  ![training_and_inference](images/resized_training_inference.png)
  
  ## Documents
  ### Dependencies
  To download all the dependencies, run the following command -    
  ` pip3 install -r requirements.txt`
  ### Directory Description
  Run the following command from the root directory to download, preprocess data, create vocab and word embeddings.    
  ` python3 setup_data.py `
  The repository should like this after running the above command -    
  <pre><code>
root
├──  images
├──  linguistic_style_transfer_pytorch
│    ├── data
│    │   ├── raw
│    │   │   ├── sentiment.train.0
│    │   │   └── sentiment.train.1
│    │   ├── clean
│    │   │    ├── yelp_train_data.txt
│    │   │    └── yelp_train_labels.txt
│    │   ├── lexicon
│    │   │   ├── positive-words.txt
│    │   │   └── negative-words.txt
│    │   ├── bow.json
│    │   ├── embedding.txt
│    │   ├── index2word.json
│    │   ├── word2index.json
│    │   └── word_embeddings.npy
│    ├── utils 
│    │    ├── __init__.py
│    │    ├── preprocess.py
│    │    ├── train_w2v.py
│    │    └── vocab.py
│    ├── __init__.py
│    ├── model.py
│    ├── data_loader.py
│    ├── config.py
│    └── checkpoints/
├──  README.md
├──  setup.py
├──  setup_data.py
├──  train.py
└──  generate.py
</code></pre>
