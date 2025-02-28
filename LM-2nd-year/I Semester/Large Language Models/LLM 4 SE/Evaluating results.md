# Evaluating LLMSE
### Evaluating requirements extraction

Precision and Recall are key metrics used to evaluate the quality of extracted requirements in **requirements engineering tasks**, especially when using LLMs to automatically generate or extract functional requirements from raw text.
1. Manually or automatically review the extracted requirements
2. Compare the extracted requirements to the ground truth
3. Calculate precision, recall and F1 score
4. Use the metrics to iterate on LLM model improvements

The **Precision** measures how many extracted requirements are actually correct:
$$
\text{Precision} = \frac{\text{True Positive (TP)}}{\text{True Positive (TP)} + \text{False Positive (FP)}}
$$
High precision means that the system generates fewer irrelevant or incorrect requirements.

**Recall** measures how many of the total relevant requirements were correctly extracted:
$$
\text{Recall} = \frac{\text{True Positive (TP)}}{\text{True Positive (TP)} + \text{False Negative (FN)}}
$$
High recall means that the system successfully identifies most of the relevant requirements, but may also include some irrelevant ones.
If we focus on requirements extraction there is no such thing as "true negative", because it's something that we don't know.

**F1 Score**: Combines precision and recall into a single metric, providing a balance.
$$
\text{F1 score} = 2 \cdot \frac{\text{Preciosion} \cdot \text{Recall}}{\text{Precison + Recall}}
$$
A high F1 score indicates a good balance between precision and recall.

An **issue** with this approach is that **different phrasing, synonyms, and contextual differences** in how requirements are written make automatic evaluation difficult. ("View trail descriptions" vs. "Browse hiking trails" vs. "See details of available trails.")
We have different solutions to this: 
- **Predefined synonym list**
- **Word embeddings**: These models represent words as vectors in a high-dimensional space, where semantically similar words are closer together. You can use these embeddings to detect words with similar meanings automatically, even if they are not exactly the same (e.g. Cosine similarity)
- Sentence embeddings
- **Text processing**; reduce each word to its "base form", for example "viewing"-> "view"

Let's consider an example with **cosine similarity**: we compare each cosine similarity with a given threshold $T$: if $\text{cs(A,B)} > T$ for a single requirement, then it means that the extracted requirement A correctly mapped to the actual one B, so this is a true positive.
If $\text{cs(A,B)} > T$ for no actual requirement, then it means that is a false positive.

![[Pasted image 20241202090721.png]]

Even if a cosine similarity is above the threshold it's possible to have a false positive, that is because cosine similarity only captures the angle between two vectors of words and not their semantic meaning in different contexts. In such cases we need:
- **Manual** review
- **Crowdsourcing**
It also could be useful to plot a **ROC curve**, which is the comparison between True Positive Rate (TPR) vs False Positive Rate (FPR).

![[Pasted image 20241202091825.png]]

### Evaluating requirements quality

Typically this is done through **questionnaires** by having humans in the loop (or, you can simulate the human reader with another LLM agent…).
**INVEST** is a quality metric that administer a questionnaires to ask whether each generated requirement or user story is:
1. Independent: checks if the requirement stands alone
2. Negotiable: Ensure it's open to change and discussion
3. Valuable: Ensure it adds tangible value to users or stakeholders
4. Estimate: Check if a requirement is clear enough for estimation
5. Small: Check if it's small and actionable
6. Testable: Check if it could be tested and validated

![[Pasted image 20241202092922.png]]

Another metric is **SRS quality measures**, that performs both per-requirement grading and document-wide grading:

| Per-requirement                                                                                                                                    | Document-wide                                                                                                                                                |
| -------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| **Unambiguous**: A requirement is unambiguous if and only if it has only one possible interpretation.                                              | **Internal Consistency**: An SRS is  internally consistent if and only if no subsets of individual requirements conflict.                                    |
| **Understandable**: A requirement is understandable if all classes of SRS readers can easily comprehend its meaning with a minimum of explanation. | **Non-redundancy**: An SRS is not redundant if no requirement is restated more than once.                                                                    |
| **Correct**: A requirement is deemed correct when it accurately represents a required feature or function the system must possess                  | **Completeness**: An SRS is complete if it details all functions, describes all<br>responses, provides organizational  clarity, and avoids placeholder text. |
| **Verifiable**: A requirement is verifiable if finite, costeffective techniques exist for verifying that it is satisfied by the system as built    | **Conciseness**: An SRS is concise when it delivers all necessary information<br>briefly without sacrificing its quality.                                    |

![[Pasted image 20241202093501.png]]
![[Pasted image 20241202093512.png]]

### Evaluating Design

In general, LLM are used for design by generating **Class, Use Case or sequence Diagrams**.
**Structural** **metrics** refer to measurements that focus on the design and architecture of a system, specifically how different components, modules, or classes are organized and interact.
A possible example is **Missing Dependencies**: A design violation occurs when a component is dependent on another component, but that dependency is not properly established or is
omitted in the system's design.
$$
\text{Missing dependency count} = \text{Total expected dependencies} - \text{Total established dependency}
$$
Other Violations could be misplaced dependencies, circular dependencies, incorrect
inheritance, or incorrect interface implementation.

In the same way we did with requirements extraction we could use precision and recall metrics.

![[Pasted image 20241202094236.png]]

### Evaluating Code generation

![[Pasted image 20241202102437.png]]

The **Functional correctness** measures the number of test cases passed by the generated code, this provides an empirical measure of "correctness", but I need in advance a test suite.
We can simply compute the **pass rate** as:
$$
\text{Pass rate} = 100 \cdot \frac{\text{Passed test cases}}{\text{Total number of test cases}}
$$
**Static Code quality measures** focus on the structure of the code without executing it. They consider structure, complexity and maintainability of the code. They provides objective assessment of whether the generated code meets industry standards for readability, scalability, and reliability.

- **Cyclomatic Complexity (CC)** measures the complexity of the generated code by counting the number of linearly independent paths through its source code. If we have a control flow graph we can calculate CC as: $$
\text{CC} = E - N +2P
$$ Where $E$ is the number of Edges, $N$ the number of nodes and $P$ the number of connected components. Once we have computed the CC we can interpret it with the following criteria:

| CC Range | Description                                                      |
| -------- | ---------------------------------------------------------------- |
| 1-10     | Simple code, easy to maintain                                    |
| 11-20    | Moderate complexity, careful review and testing                  |
| 21-50    | High complexity, refactoring needed for improving maintanability |
| 50+      | Very complex, refactor of change design                          |

- **Maintainability Index (MI)** is a metric used to assess the ease with which a software system can be **maintained**. It provides an estimate of how difficult it is to understand, test, and maintain the code. It combines several factors into a **single score** that reflects how easy it is to maintain and improve the code over time. ![[Pasted image 20241202103731.png]] A possible factor is the **Halstead Volume** computed as follows: $$
\text{HV}= (N_{1}+N_{2}) \cdot \log_{2}(N_{1}+N_{2})
$$ where $N_{1}$ is the number of distinct operators in the code, and $N_{2}$ the number of different operands. Higher Volume indicates more complexity and larger code, requiring more effort to understand and maintain. Lower Volume suggests simpler, more concise code.


**Runtime performance quality metrics**: Several key metrics can be used to assess the efficiency, scalability, and responsiveness of the generated code. These metrics focus on
how the code performs during execution, providing insight into its efficiency and identifying potential performance bottlenecks.
Some metrics are: **Execution time, Throughput, Memory consumption, CPU time used and Error rate**.
	