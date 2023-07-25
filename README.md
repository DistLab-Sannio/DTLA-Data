# A Distributed Tracing Pipeline for Improving Locality Awareness of Microservices Applications

## Introduction
This repository contains the raw data obtained during the experimentation of a framework for the generation of locality-aware placements of microservices under a defined workload via clustering techniques.

## Data
The *endpoint_op_mapping.csv* file stores the mapping between the names and urls of the REST endpoints exposed by the SUT, which in the considered case study is [Train Ticket] .

The *endpoint_to_count.csv* file, instead, contains the information about the calls that occur between the microservices composing the SUT, specifically their frequencies and durations. In particular, every endpoint, *i.e*, url, is associated with a list of tuples, each one comprising *i)* the number of hops (number of calls between pairs of microservices) and *ii)* the duration (the response time) that characterize the call represented by the tuple itself. The length of the list is the number of times the endpoint is invoked by an external client. For example the following raw      

```sh
POST:/api/v1/orderservice/order/refresh;[(2, '0.072707036s'), (2, '0.116240330s'), (2, '0.032895775s')]
```

represents an endpoint called 3 times: each invocation exhibits a different reponse time and the same number of hops.

The experimentation involved the evaluation of diverse deployment proposals, deriving from the application of different clustering approaches, in different network setups. In particular, we considered a *fast* network (round trip latency of 400 us) and a *slow* network (round trip latency of 4 ms), along with the following clustering techniques: *Spectral* (SP), *Assignment based on Reaching Centrality* (ARC), and *Locality Bounded* (LB). We also considered 3 random placements. For each configuration, we executed 3 runs.


[Train Ticket]: <https://github.com/FudanSELab/train-ticket>
