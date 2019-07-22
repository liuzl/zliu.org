+++
title = "Filtered-Space Saving Top-K"

date = 2018-06-25
lastmod = 2018-06-25
draft = false

tags = ["Algorithm"]
summary = "[Filtered-Space Saving](http://www.l2f.inesc-id.pt/~fmmb/wiki/uploads/Work/dict.refd.pdf) (FSS) is a data structure and algorithm combination useful for accurately estimating the top k most frequent values appearing in a stream while using a constant, minimal memory footprint."

+++

Before diving into **Filtered-Space Saving algorithm**, let us talk about the **Space-Saving algorithm** first, it was proposed by Metwally et al. in [1]. Space-Saving underlying idea is to monitor only a pre-defined number of `m` elements and their associated counters. Counters on each element are updated to reflect the maximum possible number of times an element has been observed and the error that might be involved in that estimate. If an element that is already being monitored occurs again, the counter for the frequency estimate is incremented. If the element is not currently monitored it is always added to the list. If the maximum number of elements has been reached, the element with the lower estimate of possible occurences is dropped. The new element estimate error is set to the estimate of frequency of the dropped element. The new element frequency estimate equal to the error plus `1`.

The Space-Saving algorithm will keep in the list all the elements that may have occurred at least the new estimate error value (or the last dropped element estimate) of times. This ensures that no false negatives are generated but allows for false positives. Elements with low frequencies that are observed in the end of the data stream have higher probabilities of being present in the list.

[Filtered-Space Saving](http://www.l2f.inesc-id.pt/~fmmb/wiki/uploads/Work/dict.refd.pdf) (FSS), that uses a filtering approach to improve on Space-Saving is a data structure and algorithm combination useful for accurately estimating the top k most frequent values appearing in a stream while using a constant, minimal memory footprint. The obvious approach to computing top-k is to simply keep a table of values and their associated frequencies, which is not practical for streams.

Instead, FSS works by hashing incoming values into buckets, where each bucket has a collection of values already added. If the incoming element already exists at a given bucket, its frequency is incremented. If the element doesn’t exist, it will be added as long as a few certain configurable conditions are met.

## References

1. A. Metwally, D. Agrawal, A. Abbadi, [Efficient Computation of Frequent and Top-k Elements in Data Streams](http://www.cse.ust.hk/~raywong/comp5331/References/EfficientComputationOfFrequentAndTop-kElementsInDataStreams.pdf), *Technical Report 2005-23, University of
California, Santa Barbara*, 2005.
2. Homem, Nuno, and J. P. Carvalho. [Finding top-k elements in data streams](http://www.l2f.inesc-id.pt/~fmmb/wiki/uploads/Work/dict.refd.pdf), *Elsevier Science Inc.*, 2010.
