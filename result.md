# Evaluation  
    
    The new AVeriTeC scoring follows a similar approach to FEVER and considers the correctness of the verdict label conditioned on the correctness of the evidence retrieved. If a claim is labelled as supported, refuted, conflicting evidence/cherry-picking, additionally the evidence will be checked against the list of annotated evidence. The label will only be considered correct if the EV2R recall score between the provided evidence and the annotated evidence is at least 0.44. The scoring script can be found on the <a href="https://fever.ai/dataset/averitec.html">AVeriTeC Dataset page</a>. A detailed explanation of the scoring metric can be found in <a href="https://arxiv.org/abs/2411.05375">this paper.</a>  
    
    For the <a href="https://arxiv.org/abs/2411.05375">new AVeriTeC score (Ev2R)</a> following changes are made to the FEVER scorer:  
    
    * Claims in Fact-Checking datasets are typically supported or refuted by evidence, or there is not enough evidence. We add a fourth class: conflicting evidence/cherry-picking. This covers both conflicting evidence, and technically true claims that mislead by excluding important context, i.e., the claim has both supporting and refuting evidence.  
    
    * Unlike in FEVER using a closed source of evidence such as Wikipedia, AVERITEC is intended for use with evidence retrieved from the open web. Since the same evidence may be found in different sources, we cannot rely on exact matching to score retrieved evidence. As such, we instead rely on approximate matching. Specifically, we use the EV2R to find an optimal matching of provided evidence to annotated evidence.  
    
    On the leaderboard, the metric to optimise for is the new AVeriTeC score (Ev2R recall). The Hungarian meteor based ones are kept for reference against the results of the 2024 shared task.  
    
    |   Q only (Hungarian meteor) |   Q + A (Hungarian meteor) |   old AVeriTeC Score (Hungarian meteor) |   Q only (Ev2R recall) |   Q + A (Ev2R recall) |   new AVeriTeC score (Ev2R recall) |
|----------------------------:|---------------------------:|----------------------------------------:|-----------------------:|----------------------:|-----------------------------------:|
|                      0.2321 |                     0.1407 |                                       0 |                      0 |                   0.5 |                                  0 |
|                      0.2321 |                     0.1407 |                                       0 |                      0 |                   0.5 |                                  0 |
