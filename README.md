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


## Project Structure Overview
The code is structured into key components, each corresponding to different stages of the QA process.</br>
All the code is located in the `Amazon Product Review QA System.ipynb` notebook.
The original project can be found on my (kaggle profile)[https://www.kaggle.com/code/seddiktrk/hf-transformers-qa/edit].

### The Dataset
📄 This project utilizes the (SubjQA)[https://huggingface.co/datasets/megagonlabs/subjqa] dataset, featuring over 10,000 customer reviews across six domains but in this project I have focused on the `Electronics` domain Each review is paired with a question that can be answered using content from the review, focusing on subjective experiences.
