|        Method            | Multi-hop Retrieval| Multi-hop Retrieval | Multi-hop Reasoning | Multi-hop Reasoning |
|--------------------------|:------------------:|:------------------:|:------------------:|:------------------:|
|                                      |        EM           |        F1           |        EM           |        F1           |
| Beam Retrieval                       |        97.63        |        98.71        |        72.62        |        85.70        |
| Coreference                          |        97.69        |        98.73        |        72.62        |        85.88        |
| T5-11B                               |        98.32        |        99.04        |        74.16        |        87.11        |
| DCE                                  |        97.89        |        98.54        |        73.33        |        86.26        |
| Vanilla prompt(Gemini-1.5-flash)     |        97.86        |        98.94        |        72.92        |        86.25        |
| ECSP (Gemini-1.5-flash)              |        98.54        |        99.15        |        74.54        |        87.28        |


> Q1: who are you 



|        Method            | SARI| BERTScore | ChrF | RougeL | BLEU | METEOR |
|----------|:----------:|:----------:|:----------:|:----------:|:----------:|:----------:|
|Few-shot ECSP(Gemini-1.5-flash) | 0.4861 |0.9446 | 0.8195 | 0.8047 | 0.6618 | 0.8317|




Thanks for your valuable suggestions.

> Q1: More baselines should be included in the final version of the paper. 
We will include all baselines we provided during the rebuttal in the final version of our paper.

> Q2: Adding discussions about the efficiency of our approach compared to vanilla prompting
We acknowledge that our method involves additional steps, including EDU segmentation, ambiguous EDU identification, relevant EDU selection and EDU decontextualisation, which inevitably leads to increased computation time during inference.

To assess the trade-off between performance and efficiency, we analyse it on the dev set of the benchmark dataset. The comparison of efficiency between our approach and Vanilla prompting is shown in the following table:

|                     Method                     | # calls | avg. inference time | SARI | BERTScore | ChrF | RougeL | BLEU | METEOR |
| Vanilla Prompt (Gemini-1.5-flash) |     1     |          0.6482           | 0.3624 |    0.9317    | 0.7462 | 0.6611 | 0.5272 | 0.7762 |
|          Ours(Gemini-1.5-flash)        |      4    |           6.6426          |  0.4858 |	 0.945     | 0.8204 | 0.8059 | 0.6611 | 0.8312 |
Table A: The comparison of efficiency between our approach and Vanilla prompting. # calls denotes the number of model calls per sample, avg. Inference time denotes the average inference time per sample.

| Method | EDU segmentation | Ambiguous EDU Identification | EDU selection | EDU Decontextualisation | Total avg. time |
|   Ours   |           4.047             |                      0.638                   |	  0.9851        |	              0.972                |       6.6426        |
Table B. The statistics of the average inference time on different modules.

As shown in Table A, while our approach has a longer inference time than vanilla prompting, our method achieves better performance across multiple evaluation metrics. Furthermore, we listed the average inference time of each module in Table B. We observe that approximately 60% of the total inference time is consumed by the EDU segmentation step, which is a sentence-level preprocessing. To improve the efficiency and reduce the computational cost of the LLMs, we plan to improve this module in the future. e.g. by replacing it with traditional methods as discussed in Table 3 of the main paper.

We will include the above discussion and analysis in our camera-ready where space allows.

