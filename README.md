# A Distributed Tracing Pipeline for Improving Locality Awareness of Microservices Applications

## Introduction
This repository contains the raw data obtained during the experimentation of a framework for the generation of locality-aware placements of microservices under a defined workload via clustering techniques.

## Data
The *endpoint_op_mapping.csv* file stores the mapping between the names and urls of the REST endpoints exposed by the SUT, which in the considered case study is [Train Ticket] .

The *endpoint_to_count.csv* file, instead, contains the information about the calls that occur between the microservices composing the SUT, specifically their frequencies and durations. In particular, every endpoint, *i.e*, url, is associated with a list of tuples, each one comprising *i)* the number of hops (number of calls between pairs of microservices) and *ii)* the duration (the response time) that characterize the call represented by the tuple itself. The length of the list is the number of times the endpoint is invoked by an external client. For example the following raw      

```sh
POST:/api/v1/orderservice/order/refresh;[(2, '0.077707036s', '0.072707036s'), (2, '0.121240330s', '0.116240330s'), (2, '0.037895775s', '0.032895775s')]
```

represents an endpoint called 3 times: each invocation exhibits a different reponse time and the same number of hops. The two times refer to the duration of a call on the API gateway (first) and on the actual public endpoint (second).

The experimentation involved the evaluation of diverse deployment proposals, deriving from the application of different clustering approaches, in different network setups and with a different number of clients. In particular, we considered a *fast* network (round trip latency of 400 us) and a *slow* network (round trip latency of 4 ms), along with the following clustering techniques: *Spectral* (SP), *Assignment based on Reaching Centrality* (ARC), and *Locality Bounded* (LB). We also considered 3 random placements (R1, R2, R3). As for the number of clients, we conducted our tests with *1*, *20* and *40* concurrent users. With *1* user, it has been possible to control the interleaving of requests. In this case, we could compare the different clustering techniques by considering them as paired. Consequently, all the measurements are reported. Instead, with *20* and *40* users, the mean and *50*-th, *95*-th and *99*-th percentiles are provided. In addition, endpoints are ranked in descending order of importance wrt their *i)* characteristic communication latency and *ii)* significance in the context of a specific workload.  

The *results_1* directory contains the data related to tests performed with *1* concurrent user. Data is divided based on the kind of network. The *clustering_eval_df.csv* file contains the results of the pairwise comparisons performed via the Wilcoxon signed-rank test between cluster-based and random deployments. In particular, for each endpoint we have the following information:

- *op_code*: operation identifier;
- *op_name*: operation name;
- *endpoint*: REST endpoint associated with the operation;
- *t_score*: rank of the operation;
- *pairwise_comparisons*: list of pairwise comparisons. For each pair, a tuple containing the effect size and p-value is reported. 
- *frequency*: number of times the operation is invoked. 

For example, the following raw

```sh
to1;{'search_travel_other_logged', 'search_travel_other_external'};{'POST:/api/v1/travel2service/trips/left'};0.7848161839738241;(96.55313796419908, 0.0);(-99.65144696610825, 1.0);(99.9995514117968, 0.0);(99.9995514117968, 0.0);(99.9995514117968, 0.0);(-96.55313796419908, 1.0);(-99.87322897377375, 1.0);(99.99470665920217, 0.0);(99.99973084707807, 0.0);(99.99838508246846, 0.0);(99.65144696610825, 0.0);(99.87322897377375, 0.0);(99.99829536482781, 0.0);(99.99856451774973, 0.0);(99.99883367067166, 0.0);(-99.9995514117968, 1.0);(-99.99470665920217, 1.0);(-99.99829536482781, 1.0);(-85.03756477613655, 1.0);(-99.60219198139615, 1.0);(-99.9995514117968, 1.0);(-99.99973084707807, 1.0);(-99.99856451774973, 1.0);(85.03756477613655, 2.502085962579139e-251);(-99.80531271980821, 1.0);(-99.9995514117968, 1.0);(-99.99838508246846, 1.0);(-99.99883367067166, 1.0);(99.60219198139615, 0.0);(99.80531271980821, 0.0);2111
```

refers to operation *to<sub>1</sub>*, whose *i)* endpoint is *'POST:/api/v1/travel2service/trips/left'*, *ii)* score is 0.785, and *iii)* frequency is 2111. The first tuple of the list relates to the comparison between SP and R1.

The *sensitivity_eval_df.csv* and *workload_eval_df.csv* have both the same format as above, with a difference in the pairs of deployments being compared. Specifically, in the first case cluster-based techniques are evaluated by considering the graph of microservices as weighted and unweighted (weights represent the number of interactions). In the second case, they are evaluated with respect to changes occurring in the workload. 

For each technique *t*, a file with suffix *response_times_df* is provided. It contains the raw response times collectied during the experiments.

The *results_20* and *results_40* directories contain the data related to tests performed with *20* and *40* concurrent users. File format is the same as the one described above. However, the tuples in the *pairwise_comparisons* column refer to a specific percentile of response time. In particular, the *clustering_eval_df_mean.csv*, *clustering_eval_df_median.csv*, *clustering_eval_df_95.csv* and *clustering_eval_df_99.csv* files consider the mean, together with *50*-th, *95*-th and *99*-th percentiles, respectively.

For example, the following raw 

```sh
to1;{'search_travel_other_logged', 'search_travel_other_external'};{'POST:/api/v1/travel2service/trips/left'};0.7848161839738241;(385.7129587873046, 509.775135954524);(385.7129587873046, 292.5351837991473);(385.7129587873046, 915.0759180483185);(385.7129587873046, 889.6038962576978);(385.7129587873046, 715.278986736144);(509.775135954524, 385.7129587873046);(509.775135954524, 292.5351837991473);(509.775135954524, 915.0759180483185);(509.775135954524, 889.6038962576978);(509.775135954524, 715.278986736144);(292.5351837991473, 385.7129587873046);(292.5351837991473, 509.775135954524);(292.5351837991473, 915.0759180483185);(292.5351837991473, 889.6038962576978);(292.5351837991473, 715.278986736144);(915.0759180483185, 385.7129587873046);(915.0759180483185, 509.775135954524);(915.0759180483185, 292.5351837991473);(915.0759180483185, 889.6038962576978);(915.0759180483185, 715.278986736144);(889.6038962576978, 385.7129587873046);(889.6038962576978, 509.775135954524);(889.6038962576978, 292.5351837991473);(889.6038962576978, 915.0759180483185);(889.6038962576978, 715.278986736144);(715.278986736144, 385.7129587873046);(715.278986736144, 509.775135954524);(715.278986736144, 292.5351837991473);(715.278986736144, 915.0759180483185);(715.278986736144, 889.6038962576978);2111
```

refers to operation *to<sub>1<sub>*, and the first tuple of the list contains the mean response time for SP and R1.

[Train Ticket]: <https://github.com/FudanSELab/train-ticket>
