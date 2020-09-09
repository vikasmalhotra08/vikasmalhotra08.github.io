---
title: 'Word2Vec and its usage in predicting the next item in your basket in an e-commerce setting'
date: 2020-08-23
permalink: /posts/2020/08/Word2Vec-E-commerce/
tags:
  - Word2Vec
  - Embeddings
  - Prod2Vec
  - Graph2Vec
  - GNN Embeddings 
---

# Product Similarities using Embeddings

Have you ever shopped at site like Amazon, Walmart or Flipkart or similar sites in your region and wondered how can these sites recommend you products similar to what you already bought or have it in your cart at a point in time? Or perhaps a product was out of stock and you were recommended a product complementary to that out of stock product?

There are various ML models at work to help predict what would be a similar item to what you already bought or what would be a complementary items to what you already have in your cart. These ML Models span the whole complexity range, starting from a simpler K-means clustering of products bought together to complex neural networks [2]. 

In this article, I will be spending time helping you understand why item similarity and complements are important in retail space, what are embeddings, how does Word2Vec, Prod2Vec and other vectorization algorithms help and looking at similar (but complex) implementations used by Airbnb's, Uber's and Google's of the world. 

## What is product similarity?

Questions of similarity are everywhere in retail:
- Your selected product X goes well with product Y.
- Your product X is out of stock, so how about this alternative Y?
- Customers like you also tend to buy Z. 

There is massive upside to solving for the above problems as they have impact on recommendations, ranking, pricing and other components of the retail space. 

Ways to solve the problems are:

1. Categorical data might get represented as one-hot encoded data set and applying any kind of similarity score will tell you that cat food and dog food are totally unrelated using this approach. 

## What are embeddings in product and e-commerce space?

In the context of any language, words can be represented as embeddings by converting them into dense vectors. These are learnt dense representations of otherwise sparse data (vector size is a parameter)

We can generate embeddings using numerous ways:
- Using BERT pre-trained models to generate embeddings for your sentences [3]
- Using Word2Vec to generate vector representation of words [4]
- Using graph neural networks to generate embeddings [7]

A general framework for generating embeddings is to build a complex multi-layer supervised model with diverse inputs and outputs. 

One of the earliest implementations to help us generate embeddings is Word2Vec. It's computational efficiency, scalability and the `skipgram with negative sampling architecture` [4] makes it special. Word2Vec paved the way to other popular models like Node2Vec, Graph neural networks, Transformers, FastText and BERT. 

If you look at a novel neural network architecture called `transformer`[8] which has the benefits of modeling long term dependencies among tokens in temporal sequence, and the more efficient training of the model in general by eliminating the sequential dependency on previous tokens. BERT, based on Transformer architecture can generate token, sentence and positional embeddings. 

## How are embeddings used across the tech landscape?

A number of companies have jumped on the bandwagon of using embeddings in their workflow and have led to promising results. `Airbnb's` use of embeddings for home listings and its use in search ranking is an interesting read [5]. `Instagram's` explore feature uses account embeddings to more efficiently identify which accounts are topically similar to each other [6]. `Uber's` use of user-based embeddings for recommendations in Uber Eats app showed how graph neural networks is the next frontier for tech companies to explore [7]. 

Some of the landmark papers for non-NLP adaptations of Word2Vec are: Prod2Vec[8], Meta-Prod2Vec[9] and Graph2Vec[7]. You can embed any object as long as you can define the respective context/environment for the object; whether sequential (E.g. search logs) or a graph. The ability to correctly define your context[10] is the most crucial part of your model, followed by your parameters in the model.  

I would be creating separate posts on paper understanding for some of the papers in this blog post. So, be on a lookout for those posts. 

## Example implementation of generating product embeddings from Instacart Orders

Given the information presented in this blog post so far and in the papers listed in resources section, I wanted to showcase how to generate embeddings from a sample database of Instacart of orders. The embeddings that we get from the sample database does not have side information and we are only embedding collaborative filtering semantics inherent in dataset into embedding vectors. In a production setting, the embeddings generally form a retrieval stage model (vector similarity search) and further use selected embeddings as input to a more complex ranking model with additional input features (Retrieval/ranking models[11]).

In Word2Vec, the `skipgram with negative sampling architecture` [4] uses two different vectors for each object: the input and output vectors. Glove [12] uses single vector in downstream tasks by averaging the input and output vectors. 

Word2Vec and other models [13,14,15,16,17] use a dual embedding space architecture which allows us to evaluate the probability of a product being in a target or context environment. When it comes to calculating the product similarity, we do it with a similarity metric like cosine similarity in Input vectors. For calculating the product complementarity, we can do it with cosine similarity between input and output vectors. 

### Understanding conventions in Instacart orders vs NLP setting

In the world of orders from Instacart, we would re-define the `words` as `products` and `sentences` as `cart`. The context for a product is the other products present in the cart. For a product `beard gel`, consider the case where we use the same vector for target(In) and context(out).


### Resources

2. Deep Interest Evolution Network for Click-Through Rate Prediction https://arxiv.org/pdf/1809.03672.pdf
3. https://mccormickml.com/2019/05/14/BERT-word-embeddings-tutorial/#33-creating-word-and-sentence-vectors-from-hidden-states
4. https://machinelearningmastery.com/develop-word-embeddings-python-gensim/
5. Airbnb's listing embeddings in search ranking: https://medium.com/airbnb-engineering/listing-embeddings-for-similar-listing-recommendations-and-real-time-personalization-in-search-601172f7603e
6. Instagram's Explore Account Embeddings: https://ai.facebook.com/blog/powered-by-ai-instagrams-explore-recommender-system/
7. Uber Eats: Using Graphs to recommend dishes. https://eng.uber.com/uber-eats-graph-learning/
8. Prod2Vec: E-commerce in Your Inbox: Product Recommendations at Scale ( https://arxiv.org/abs/1606.07154) 
9. Meta-Prod2Vec: Product Embeddings Using Side-Information for Recommendation (https://arxiv.org/abs/1607.07326)
10. Real-time Personalization using Embeddings for Search Ranking at Airbnb ( https://www.kdd.org/kdd2018/accepted-papers/view/real-time-personalization-using-embeddings-for-search-ranking-at-airbnb)
11. Deep Neural Networks for YouTube Recommendations ( https://ai.google/research/pubs/pub45530)
12. GloVe: Global Vectors for Word Representation ( https://nlp.stanford.edu/projects/glove/)
13. Inferring Complementary Products from Baskets and Browsing Sessions ( https://arxiv.org/pdf/1809.09621.pdf)
14. Complementary-Similarity Learning using Quadruplet Network ( https://arxiv.org/abs/1908.09928)
15. Inferring networks of substitutable and complementary products ( https://arxiv.org/abs/1506.08839)
16. Context-aware Dual Representation Learning for Complementary Products Recommendation ( https://arxiv.org/pdf/1904.12574.pdf)
17. Exponential Family Embeddings ( https://arxiv.org/abs/1608.00778)