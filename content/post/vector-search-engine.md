+++
title = "Vector Search Engine"

date = 2022-03-01
lastmod = 2022-03-01
draft = false

tags = ["Vector", "Search Engine", "Database", "Information Retrieval"]
summary = "Vector search engines or vector databases are a core piece of infrastracture that fuels every big deep learning deployment in industry."

+++

In deep learning era, suddenly everything was being represented as these high-dimensional vectors, that quickly became a huge source of data, so it you want to search, rank or give recommendations, the object in your actural database wasn't a document or an image -- it was this mathematical representation of the deep learning model. In short, this quickly became important for a lot of companies.

As a result, vector search engines or vector databases became a core piece of infrasctracture that fuels every big deep learning deployment in industry.

## Vector search changes business

Vector search is not only applicable to image and text content. It can also be used for information retrieval for anything you have in your business when you can define a vector to represent each thing. Here are a few examples:

* Finding similar users: If you define a vector to represent each user in your business by combining the user’s activities, past purchase history, and other user attributes, then you can find all users similar to a specified user.  You can then see, for example, users who are purchasing similar products, users that are likely bots, or users who are potential premium customers and who should be targeted with digital marketing.

* Finding similar products or items: With a vector generated from product features such as description, price, sales location, and so on, you can find similar products to answer any number of questions; for example, "What other products do we have that are similar to this one and may work for the same use case?" or "What products sold in the last 24 hours in this area?” (based on time and proximity)

* Finding defective IoT devices: With a vector that captures the features of defective devices from their signals, vector search enables you to instantly find potentially defective devices for proactive maintenance.

* Finding ads: Well-defined vectors let you find the most relevant or appropriate ads for viewers in milliseconds at high throughput.

* Finding security threats: You can identify security threats by vectorizing the signatures of computer virus binaries or malicious attack behaviors against web services or network equipment. 

* ...and many more: Thousands of different applications of vector search in all industries will likely emerge in the next few years, making the technology as important as relational databases.

OK, vector search sounds cool. But what are the major challenges to applying the technology to real business use cases? Actually there are two:

* Creating vectors that are meaningful for business use cases
* Building a fast and scalable vector search service

## Embeddings: meaningful vectors for business use cases

todo

## Building a fast and scalable vector search service

todo

## References

1. Facebook. [A library for efficient similarity search and clustering of dense vectors.](https://github.com/facebookresearch/faiss), Feb 19, 2017.
2. Zilliz. [An open-source vector database for scalable similarity search and AI applications.](https://github.com/milvus-io/milvus), Mar 17, 2019.
3. Microsoft. [A distributed approximate nearest neighborhood search (ANN) library which provides a high quality vector index build, search and distributed online serving toolkits for large scale vector search scenario.](https://github.com/microsoft/SPTAG), Sep 9, 2018.
4. Semi-technologies. [Weaviate is a cloud-native, modular, real-time vector search engine](https://github.com/semi-technologies/weaviate), Mar 27, 2016.
5. Qdrant. [Vector similarity search engine with extended filtering support](https://github.com/qdrant/qdrant), May 24, 2020.
6. Pinecone. [Vector Database for Similarity Search](https://www.pinecone.io/), Apr 18, 2021.
7. Vald. [A Highly Scalable Distributed Vector Search Engine](https://github.com/vdaas/vald), Aug 25, 2019.

