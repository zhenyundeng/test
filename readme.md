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

