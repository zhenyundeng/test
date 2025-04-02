# Reviewer 6EkM
We thank the reviewer for the valuable comments. Detailed responses to each specific comment are provided below.

**W1**: *The method heavily relies on the quality of AMR parsing. Errors in parsing or entity linking could propagate to subgraph retrieval and reasoning.*

Unlike iterative subgraph expansion methods such as PullNet [1] or SR [2], which rely on multi-hop traversal from the topic entity and are thus prone to compounding errors during expansion. Instead, our method uses AMR to impose global semantic constraints that guide subgraph retrieval. This design inherently mitigates the propagation of parsing or linking errors throughout the graph.

Moreover, we adopt SPRING [3], a state-of-the-art AMR parser, in combination with BLINK [4] for entity linking. These tools have been shown to perform robustly across multiple NLP tasks. As shown in Table 2 and Table 3, our method achieves notable improvements in both quality and QA accuracy on the WebQSP and CWQ datasets. These results demonstrate that the current quality of AMR parsing is sufficient for practical KBQA applications.

**W2**: *The paper does not explicitly discuss how the method scales to extremely long or nested AMR paths.*

While real-world questions can indeed yield long or nested AMR paths, our method is explicitly designed to handle such complexity effectively. As described in Section 3.2 and illustrated in Figure 2 (AMR Graph → AMR Relationships), we avoid treating the AMR path as a monolithic structure. Instead, we decompose it into a sequence of predicate-argument units, each representing a localized semantic relation.

This decomposition enables the system to incrementally isolate and extract meaningful relations, thereby minimizing the impact of overall path length or structural depth. Consequently, our approach remains robust and scalable when dealing with extended reasoning chains and deeply nested semantic representations.

**W3**: *The additional steps may introduce latency, but runtime efficiency is not evaluated.*

While our method introduces additional steps, such as AMR parsing and semantic relation extraction, the overall computational cost of the subgraph retrieval module is relatively low compared to the reasoning module. In our implementation, the runtime bottleneck primarily lies in the reasoning stage, which aligns with prior findings in KBQA literature (e.g., SR [1], NSM [5]).

Moreover, our method produces more compact subgraphs by filtering out semantically irrelevant nodes and relations (see Table 2). This not only improves subgraph quality but also reduces the reasoning workload, resulting in better overall efficiency.

**Comment 1**:  *The paper mentions "noisy nodes" in baseline methods but does not quantify noise reduction in the proposed method.*

We follow the evaluation protocol introduced in SR [1], where subgraph quality is assessed through three interrelated metrics: subgraph size, answer coverage rate, and final QA performance. Specifically, Table 2 reports the number of nodes and relations in the retrieved subgraphs, while Table 3 shows the answer coverage and QA accuracy.

Compared to baseline methods, our approach produces significantly smaller subgraphs while maintaining or improving answer coverage and QA performance. This indicates that our method effectively reduces semantically irrelevant or noisy nodes, without sacrificing retrieval completeness. We acknowledge that providing an explicit definition and quantification of “noise” could enhance clarity, and we plan to explore this direction in future work.
 
[1] PullNet: Open Domain Question Answering with Iterative Retrieval on Knowledge Bases and Text. arXiv 2019.  
[2] Subgraph Retrieval Enhanced Model for Multi-hop Knowledge Base Question Answering. ACL 2022.  
[3] One SPRING to Rule Them Both: Symmetric AMR Semantic Parsing and Generation without a Complex Pipeline. AAAI 2021.  
[4] Scalable Zero-shot Entity Linking with Dense Entity Retrieval. EMNLP 2020.  
[5] Improving Multi-hop Knowledge Base Question Answering by Learning Intermediate Supervision Signals. ACM 2021.

# Reviewer PHMs

**W1**: *This paper does not compare with LLM-based approaches.*

While we acknowledge the recent progress in LLM-based QA systems, our method addresses a complementary problem. Specifically, our focus is on high-quality subgraph retrieval over structured knowledge graphs using semantic signals from AMR graphs. In contrast, most LLM-based methods, such as those based on retrieval-augmented generation (RAG), emphasize text generation over unstructured or semi-structured sources, and do not explicitly retrieve or evaluate graph substructures.

Additionally, our approach is model-agnostic and can be integrated with LLMs. For example, the AMR-constrained subgraphs produced by our method could be used as an external context for LLMs, offering better control and interpretability in multi-hop reasoning. We agree that direct comparisons or integrations with LLM-based KBQA methods (e.g., with tool-augmented prompting) are promising directions, and we plan to explore them in future work.

**W2**: *Paper Structure and Typos*

We thank the reviewer for the constructive suggestions. We will improve the structure and flow of the paper, enhance figure and algorithm annotations, and correct typos throughout the manuscript.

**W3**: *Missing Baselines (NuTrea, RAH-KBQA)*

We appreciate the reviewer’s suggestion to compare with recent advances such as NuTrea [1] and RAH-KBQA [2]. These methods focus on improving the reasoning process over a given subgraph, employing neural tree search or dual-graph representation to better capture entity-relation interactions. In contrast, our work addresses a different stage of the KBQA pipeline—the retrieval of high-quality subgraphs using AMR-guided semantic matching. Notably, both NuTrea and RAH-KBQA assume the subgraph is pre-extracted from the seed entity using simple neighborhood expansion strategies. Their papers do not provide detailed descriptions or learning-based designs for subgraph retrieval, nor do they explicitly evaluate subgraph quality.

We believe our approach is complementary to these methods. The AMR-driven subgraph retrieval technique we propose could serve as a plug-in front-end to improve the reasoning accuracy of such models by providing cleaner, semantically aligned subgraphs. We plan to explore such integrations in future work.

**W4**:  *Subgraph Quality, Efficiency, and Ground-Truth Coverage*

We appreciate the reviewer’s suggestion to report the inclusion ratio of ground-truth relations. However, existing datasets such as WebQSP and CWQ do not provide gold-labeled reasoning paths or relation sequences. While it is common practice to approximate ground-truth paths using the shortest paths from the topic entity to the answer entity, this heuristic does not always reflect the true reasoning logic needed to answer the question, as we discuss in Section 3.5.

In our evaluation, we follow the metrics commonly used in prior work such as SR [3], including subgraph size, answer coverage, and QA accuracy to assess subgraph quality. As shown in the results, our method significantly reduces subgraph size while improving QA performance and maintaining reasonable answer coverage, which collectively demonstrates the effectiveness of our retrieval approach.

As for inference efficiency, although we do not report explicit runtime figures, our method employs the same reasoning model as the baselines. Because our retrieved subgraphs are significantly smaller, the reasoning component processes fewer nodes and edges, resulting in shorter inference time under equivalent model configurations.

**W5**:  *Implementation Details*

We acknowledge the need for more implementation details. In the final version, we will provide a comprehensive description of the key components, including AMR parser configurations, relation extraction procedures, similarity threshold settings, and training parameters. We will also release the code and datasets to support reproducibility.

**Comment 1**: *Clarity and Writing Issues*

We will revise Section 2 to better highlight the differences between our method and prior AMR-based approaches, particularly in how we leverage AMR to constrain subgraph retrieval rather than directly generating symbolic query graphs.

We agree that the current title of Section 4.3 may be ambiguous, and we will revise it to explicitly refer to reasoning evaluation to distinguish it from subgraph retrieval.

We would also like to clarify that Section 3.2 already describes the generation and purpose of AMR paths. Specifically, the AMR path refers to the unique and deterministic path in the AMR graph from the topic entity to the amr-unknown node. This path serves two key purposes: it guides the construction of semantic relations used in subgraph retrieval, and it also supports the formulation of reasoning chains. The key distinction between AMR paths and the reasoning chains described in Section 3.5 lies in their source: AMR paths are derived from the AMR parsing of the input question, whereas the reasoning chains are composed of entities and relations mapped from the knowledge graph.

In addition, we will add clarifying annotations to Algorithm 1 to specify the types and roles of variables such as $e_1$, $e_2$, and $e_3$, and will elaborate on the definitions and roles of variables in Equations (2)–(4).

Finally, we will correct the identified typos in lines 229 (“qestions”), 326 (“parameter”), and 415 (“$T_A$, P”), and thoroughly proofread the manuscript for other minor issues.

[1] NuTrea: Neural Tree Search for Context-guided Multi-hop KGQA. NIPS 2023.  
[2] Relation-Aware Question Answering for Heterogeneous Knowledge Graphs. EMNLP Findings 2023.  
[3] Subgraph Retrieval Enhanced Model for Multi-hop Knowledge Base Question Answering. ACL 2022.  

# Reviewer z1Pc

**W1**:  *Performance Gap with EPR and AMR Dependency*

We thank the reviewer for pointing out the performance gap between our method and EPR on the CWQ dataset. We acknowledge this difference and appreciate EPR’s strong performance on complex multi-hop questions. However, we note that on the WebQSP dataset, our method achieves higher QA accuracy than EPR (see Table 3), demonstrating the effectiveness of our AMR-guided approach in simpler question settings.

EPR’s advantage on CWQ may stem from its direct modeling of structural dependencies within the knowledge graph through the construction and ranking of evidence patterns, which allows it to retain fine-grained fact structures during reasoning. In contrast, our approach focuses on guiding subgraph retrieval using AMR-derived semantic structure from the input question.

While the two methods differ in methodology and scope, they share a similar intuition—constructing structured semantic patterns to improve subgraph quality. Based on this connection, we will consider the potential of leveraging AMR to construct structural patterns directly within the knowledge graph, as a means to support more precise reasoning over complex questions.

Regarding the dependency on AMR parsing, we acknowledge that the quality of semantic parsing can influence downstream performance. To mitigate this, we adopt SPRING [1], a state-of-the-art AMR parser, which has demonstrated strong robustness across various benchmarks. Furthermore, unlike traditional KGQA methods that iteratively expand the graph and risk error propagation, our method uses AMR-derived structure to semantically constrain subgraph retrieval. This design helps reduce the impact of local parsing errors. The overall stable performance on both WebQSP and CWQ supports the practical reliability of current AMR parsers for KBQA tasks.

**Comment 1**:  *Reasoning Chain Rationality*

We thank the reviewer for raising this important question. In current KBQA benchmarks such as WebQSP and CWQ, gold-labeled reasoning chains are typically not provided and are difficult to obtain, which limits the feasibility of direct quantitative evaluation.

Our reasoning chains are constructed based on AMR-derived paths, which inherently preserve the semantic structure of the question. These paths not only align with the core predicate-argument structure of the input but also guide the expected types of entities appearing along the reasoning path, thereby improving interpretability and semantic consistency.

While our current evaluation focuses on QA accuracy, we recognize the value of directly assessing reasoning chain quality. In future work, we plan to conduct human evaluations to assess the semantic coherence and correctness of the generated reasoning chains.

**Comment 2**: *Typos*

We appreciate the reviewer’s attention to detail. We will correct the identified typos in Section 3 (“whichpredicts”), Section 3.3 (clarifying the expression “any subgraph G = ...”), and Section 4.1 (adjusting the section title formatting). We will also carefully proofread the full manuscript to ensure consistency and clarity in language and presentation.
 
[1] One SPRING to Rule Them Both: Symmetric AMR Semantic Parsing and Generation without a Complex Pipeline. AAAI 2021.
