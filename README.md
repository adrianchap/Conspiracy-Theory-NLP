# Conspiracy-Theory-NLP

## BERT Topic Modeling and GPT-2 Finetune & Application Pipeline
by Adrian Chapman

This repository contains the project materials necessary to:

 - download a large corpus of text data from reddit.com using Google BigQuery
 - explore the corpus through topic modeling with BERT
 - finetune a GPT-2 model on the corpus to generate novel text similar to the reddit posts
 - launch a simple Streamlit application to prompt the finetuned GPT-2 model from Colab using a free GPU

 This project used data from r/conspiracy as an investigation into the nature of msinformation on the internet.  However, another user could use the same pipeline on any subreddit of their choosing.

 The bulk of the pipeline consists of 3 Google Colab Notebooks:

 1. [conspiracy_topic_model.ipynb](https://colab.research.google.com/drive/1lwcimbIShuoxvubDGSHi27J0H8kGpCXr#scrollTo=kQGutHSVXOkp) for topic modeling with BERT
 2. [conspiracy_gpt2_finetune.ipynb](https://colab.research.google.com/drive/1HAxgK7oFBfUWum8lJaKKpbds799rG1iE#scrollTo=oljB7fQUoWlh) for finetuning GPT2
 3. [conspiracy_streamlit_app.ipynb](https://colab.research.google.com/drive/1C76cuHwTEQcZ5DZTaQ-INDPl9Ds3l1oE#scrollTo=1OL5WxH4YimM) for launching the Streamlit text generation application

 There are also some helper notebooks in the repository:

 1. *pull_data_pushshift.ipynb* contains a function to pull large amounts of data from reddit as an alternative to using Google Big Query
 2. *stich_large_dataset.ipynb* has code to combine several datasets downloaded from Google BigQuery
 3. *EDA_on_text.ipynb* for simple EDA on the text


### Data:

Over 30,000 titles from Reddit posts from r/conspiracy pulled from Google BigQuery using the code below.  Minimal text cleaning is applied to the SQL query, and only posts with 50 or more comments were considered:

<pre>
    <code>
        SELECT
            REGEXP_REPLACE(REGEXP_REPLACE(REGEXP_REPLACE(REGEXP_REPLACE(title, '&amp;', '&'), '&lt;', '<'), '&gt;', '>'), '�', '') AS title, num_comments, score, created_utc
        FROM
            `fh-bigquery.reddit_posts.*`
        WHERE 
            _TABLE_SUFFIX BETWEEN '2016_01' AND '2019_12'
            AND LENGTH(title) >= 10
            AND LOWER(subreddit) = 'conspiracy'
            AND num_comments >= 50

    </code>
</pre>
  
  
Depending on the size of the data pull, it may need to be downloaded in several sections and stitched together using the *stitch_large_dataset.ipynb.  Just change the _TABLE_SUFEX_ BETWEEN clause to pull smaller date ranges before downloading.

After the data has been downloaded and concatenated, the resulting .csv file can be used in the topic modeling and gpt-2 finetuning pipelines.