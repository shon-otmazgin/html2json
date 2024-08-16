# HTML2JSON: Extracting and Classifying Data from HTML Tables

## Definition
Customer data is often presented in HTML tables, which typically include:
- Table Header
- Table Body (rows, columns, and their content)
- Table Footer

**Example Table:**

**Table 26.94.94.95.44.47 Curator**

|                 | Jacob Hunter | Kendra Jackson | Jennifer Brown | Mrs. Jamie Alvarez DDS |
|-----------------|--------------|----------------|----------------|------------------------|
| Blair and Sons  | 1285         | 1242           | 1265           | 999                    |
| Wilkerson Group | 1328%        | 758            | 759%           | 958                    |
| Wilson-English  | 557%         | 1306%          | 1109           | 294%                   |
| White LLC       | 1309         | 1171           | 1459           | 1206                   |
| Thomas-Rogers   | 492%         | 1475%          | 50%            | 1489                   |

**Creation Date:** 29 Apr 2016, Vietnam

## Problem Statement
We aim to perform information extraction from structured HTML tables related to clinical studies. Given the variety of table types, we need to classify each cell based on its group or category.

## Proposed Solution

### Entity Definition
Predefine a set of entities for extraction and classification from HTML tables. This set can be customized based on specific customer needs or activities. For instance, in the provided example, potential entities include:
1. `table_id`
2. `creation_date`
3. `headers` (rows/columns)
4. `content`

### Annotation Strategy
Annotate each HTML table with the predefined entity categories using the BIO tagging scheme. This approach is commonly used in NLP for sequence labeling tasks and offers several advantages:
1. **Boundary Identification:** Accurately marks the beginning and end of entities, minimizing ambiguity.
2. **Entity Distinction:** Differentiates between the start and continuation of multi-word entities.
3. **Model Training:** Provides structured supervision such as entity positions to enhance model accuracy.
4. **Handling Complex Cases:** Effective for adjacent, nested, or overlapping entities, extremely important for tables.

**Annotation Example:**
- `Table` **O**
- `26.94.94.95.44.47` **B-table_id**
- `Curator` **O**

### Dataset Creation
Given the dataset **provided** consisting of pairs of `(HTML table, metadata JSON)`, we will convert the HTML table sequences into BIO HTML table sequences.

**Generalization Process**: To create a dataset for more complex tables tailored to different customers or activities, follow these steps:
1. **Select a sample** of 10-50 HTML tables as a test set.
2. **Annotate these tables** using human experts.
3. **Annotate the same tables** using an LLM (such as OpenAI or Anthropic) with a guided annotation scheme and instructions.

Next, assess the agreement (accuracy) between the human expert annotations and the LLM annotations:

- If the agreement is high (e.g., >90%), you can use the LLM to annotate additional tables to build a larger dataset.
- If the agreement falls between 70% and 80%, you may need to refine the instructions and/or provide a few-shot set of examples from human experts to improve the LLM's performance.
- If the LLM’s performance remains inadequate, human annotation will be necessary for creating the dataset.
### Dataset Splits
Divide the dataset into three parts:
- Training Set
- Development Set
- Test Set

### Model Architecture
1. **BERT-like Encoder:** processes input text tokens and output contextual information as vector embedding.
2. **Classification Head:**  on top of the BERT encoder, an MLP layer to classify each vector embedding into multi class classification.
3. **Loss Function:** Calculation with cross-entropy loss for each token.

### Evaluation Metrics
Assess model performance using Precision, Recall, and F1-Score to ensure accurate classification of table elements.
