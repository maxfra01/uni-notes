# LLM for Software Engineering

**LLMSE**: integration of Large Language Models (LLMs) into software engineering. It encompasses any application where the products or processes leverage LLMs to enhance development and operational efficiency.

**LLM Application**: Defined as any task or activity that benefits from LLM insights. This broad definition captures the essence of LLM's versatility across various domains, offering improvements through its advanced computational capabilities

**LLM Consumer**: Any individual, system, or process that utilizes LLM outputs. This definition acknowledges the wide array of LLM beneficiaries, from developers and businesses to automated systems, all relying on LLM-generated intelligence.

**Assured LLMSE**: This innovative approach guarantees the reliability of LLM outputs. Every response from an LLM, possibly after undergoing post-processing, comes with a verifiable assertion of its usefulness. Assured LLMSE sets a standard for trust and quality in the application of LLMs in software engineering.

Nowadays, more than 70 LLMs have been used for SE tasks. Choosing the right LLM is a crucial task: for example, if we need to do a task focused on understanding something, it's preferred to choose a encoder-only model which has better understanding capabilities.

| Model           | Type                         | Example of tasks                                          |
| --------------- | ---------------------------- | --------------------------------------------------------- |
| Encoder-only    | Understanding                | Code understanding, Bug location, Vulnerability detection |
| Encoder-decoder | Understanding and generation | Code summarization, code translation, program repair      |
| Decoder-only    | Generation                   | Code generation, code completions, Test case generation   |

Key factors guiding the selection of the type of the model are:
- Model **proficiency** in understanding the **context** of the code
- Ability to **generate relevant** content
- Responsiveness to **fine-tuning**
- Demonstrated performance in **SE benchmark**

Let's now focus on the process of fine-tuning a model for SE tasks, and more in particular, on the types of dataset.

# Datasets

Four different types of datasets could be employed to fine-tune a model:
- **Open source datasets**: publicly accessible collections of data
- **Collected datasets**: datasets compiled from the researchers directly from a multitude of sources, including (but not limited to) major websites, forums, blogs...
- **Constructed datasets**: specialized datasets that researchers create by modifying or augmenting collected datasets to better align with their specific research objectives
- **Industrial dataset**: obtained from commercial or industrial entities; often contain proprietary business data, user behavior logs, and other sensitive information.

![[Pasted image 20241111105445.png]]

We can clearly see that the open-source datasets are the most used for research papers. That is because of their credibility and authenticity, and they often contain real-world data collected from various sources.

Regardless of the origin of the datasets, we can classify datasets in:
- **Text based datasets**: composed by textual elements or code snippets, among text-based datasets there are programming tasks/problems, the most frequently used of all data types. This dominance can be attributed to the diverse and challenging nature of programming problems.
- **Code based datasets**: Only composed by code artifacts. Among code-based datasets, the predominant ones are repository of production source code. This predominance is due to the fundamental role in SE, since source code serves as the foundation of any software project. Other datasets have buggy code.
-  **Graph-based datasets**: can be used when representing the GUI states of an application to develop or test. An example is the use of screenshot from Google Play Android to Construct a graphical user interface (GUI) repository.
-  **Software repository based datasets**: compilations of data extracted from version control systems, such as Git repositories, containing code, documentation, and related artifacts. This data includes code repository, issues and commits, and so on.
- **Combined datasets

Since we are dealing with various format, **the data types** (i.e. the type of the dataset) **will influence the pre-processing technique** that will be applied.

Some example of datasets: MBPP (text-based)

