# Amazon Product Review QA System

## Descrption
<p style="background-color:#e6f7ff; 
          padding:15px; 
          color:#111;
          font-size:16px;
          border-width:3px; 
          border-color:#d0eefc; 
          border-style:solid;
          border-radius:6px"> 🔍 This project presents a hybrid Question Answering <code>(QA)</code> system for answering questions about Amazon products based on customer reviews, delivering a system for product insights and customer support.</br>
          The project involves retrieving relevant documents to the user's query and extracting answers from them, commonly through <code>extractive QA</code>, where the answer is identified as a span of text within a document, or by leveraging <code>generative QA</code> where a model generates an answer based on the retrived docs and the query.
          In the last section of the project i have experimented with the current state of the art generative QA <code>RAG</code>.
RAG extends the classic retriever-reader architecture by swapping the reader for a generator.
</p>

## Visuals
```python
item_id = "B0074BW614" # corresponds to one of Amazon’s Fire tablets
query = "Is it good for reading?"
```

<div style="
    background-color: #fff6ff;
    color: #111;
    font-size: 16px;
    padding: 15px;
    border-width: 3px;
    border-color: #efe6ef;
    border-style: solid;
    border-radius: 6px;
            ">
  📚 <code>ChatGPT</code> response given the <code>prompt</code> containing the docs and the query:
    <br><br>
    <p style="
    background-color: #555;
    color: #fff;
    font-size: 14px;
    padding: 15px;
    border-width: 2px;
    border-color: #111;
    border-style: solid;
    border-radius: 6px;"
        >
֎ &nbsp; &nbsp;Yes, the Kindle Fire is considered <code>good for reading</code>. Users appreciate the crisp and clear colors, which enhance the reading experience, and the device's light weight makes it comfortable to hold. However, some users note that the battery life may not last long and that the screen can glare in bright sunlight. Overall, many find it suitable for reading, though individual comfort may vary, especially for those with physical limitations.</p>
</div>

</br>

## Project Structure Overview
The code is structured into key components, each corresponding to different stages of the QA process.</br>
All the code is located in the `Amazon Product Review QA System.ipynb` notebook.
The original project can be found on my (kaggle profile)[https://www.kaggle.com/code/seddiktrk/hf-transformers-qa/edit].
The `QA` system is built using the `Haystack` library, using several of its components: the retriever,the document store and the reader.</br>

</br>

### The Dataset
📄 This project utilizes the [SubjQA](https://huggingface.co/datasets/megagonlabs/subjqa) dataset, featuring over 10,000 customer reviews across six domains but in this project I have focused on the `Electronics` domain Each review is paired with a question that can be answered using content from the review, focusing on subjective experiences.

</br>

### The Retriever

We utilize an `ElasticsearchDocumentStore` to store and manage product reviews, this DB enables quick retrieval of relevant information based on user queries and is compatible with both Dense and sparse retrievers.
Since users will only provide their questions without context, the retriever is essential for identifying and filtering pertinent documents from the review database and in this project I have experimented with two different types of retrivers `BM25` and `DPR`.For the DPR part i have used `facebook/dpr` models.

By combining the document store with the retriever, we streamline the process of extracting meaningful insights from large volumes of data.

**example usage**
```python
item_id = "B0074BW614" # corresponds to one of Amazon’s Fire tablets
query = "Is it good for reading?"

retrieved_docs = bm25_retriever.retrieve(
                 query=query,
                 top_k=3,
                 filters={"item_id":[item_id],
                          "split":["train"]})
```
**result**
```python
{'content': 'I never thought I would want a fire for I mainly use it for book reading.  I decided to try the fire for when I travel ...',
 'content_type': 'text',
 'score': 0.6857824513476455,
 'meta': {'item_id': 'B0074BW614',
  'question_id': '868e311275e26dbafe5af70774a300f3',
  'split': 'train'},
 'id_hash_keys': ['content'],
 'embedding': None,
 'id': '4a6aa9c7808ebba8d35aeecbcc3c30fe'}
```
</br>

### The Reader
In the system, I initially focused on extracting answers from customer reviews by identifying text spans that contain relevant information.
This task is framed as a span classification problem, where the model predicts the start and end tokens of the answer within the review text.
For the extractive QA, I have opted for the `MiniLM` model fine-tuned on the SQuAD dataset for quick exploration of various techniques.

I have also incorporated the retrieval-augmented generation `(RAG)` technique.
In this setup, we replace the traditional reader with an LLM to generate answers, in this case I have used the `ChatGPT`.
The system leverages the relevant documents based on user's queries and combines them with the query into a structured prompt. This enables the LLM to synthesize information from multiple passages, producing coherent and contextually rich responses tailored to the user’s question.

</br>

### Evaluation
#### Evaluating the Retriever
To evaluate our retriever, we focus on the metric of `recall`, which measures the fraction of relevant documents retrieved by the system. In this context, a document is deemed relevant if it contains the answer to a given question. We evaluated both the BM25 and DPR retrievers, achieving nearly perfect recall scores of 0.99 at 
𝑘=10.

#### Evaluating the Reader
In the evaluation of the extractive reader, we employed two key metrics: Exact Match `EM` and `F-score`.
The EM metric assesses whether the predicted answer matches the ground truth answer exactly, returning a score of 1 for an exact match and 0 otherwise. 
In contrast, the F-score provides a more lenient measure by calculating the harmonic mean of precision and recall.
| Metric             | Score |
|--------------------|-------|
| Exact Match (EM)   | 0.49  |
| F1 Score           | 0.52  |

</br>

## Conclusion
In this project, I have developed a QA system designed to answer customers question based on Amazon product reviews. I have explored various techniques and experimented with various tecnologies. 

Looking ahead, there are numerous opportunities to enhance the capabilities of our system. Scaling the project with larger datasets, such as the (Amazon reviews dataset)[https://huggingface.co/datasets/McAuley-Lab/Amazon-Reviews-2023] containing millions of reviews, could significantly improve the system's performance.
Additionally, integrating advanced techniques for RAG, such as `query expansion`, `reranking` of retrieved documents, and implementing chat `memory` for contextual understanding, can further enhance the performance of the system and the user experience as well.
