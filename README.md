# Reddit_Sentiment_Summary
Classify posts in the r/Crypto.com reddit thread into positive, neutral and negative sentiment, and get a summary of the opinions towards Crypto.com products

## Problem statement
Customers post opinions on reddit threads. The public platform is an important source of feedback for continuous improvement to product offering. It can be time-consuming to keep track of the conversations on the reddit thread.

## Goal
To write a code that can automate the aggregation of latest posts on the thread, understand the sentiment and get a concise overview of the positive, neutral and negative feedback. Time spent on running the code and getting output should be < 5 mins. 

## Data collection
There are multiple webscraping methods, here I use the `Request` python library. I reuse the code from an [old repository](https://github.com/els-p/Bot_or_Human/blob/master/codes/0.1%20Scraper.ipynb) and adapt it to also scrape the content of the post (not only the title). Because I will use openai API summarizer which has a maximum context length of 4097 tokens, I only iterate through 5 pages of 25 posts in the thread to form the dataset. 

## Data processing
Posts with images, httpsreddit.com links are excluded from the dataset. 

## Modelling
[This roBERTa-base model](https://huggingface.co/cardiffnlp/twitter-roberta-base-sentiment) trained on ~58M tweets and finetuned for sentiment analysis with the TweetEval benchmark was shared on Hugging Face. I used it for sentiment analysis and label each post as positive, neutral or negative. For a summary of each sentiment group, I asked ChatGPT to "Write a simple code using openai api to summarise a corpus" and "how to improve accuracy of the output", then used ChatGPT's recommendation to summarise the dataset. See our [conversation](https://chat.openai.com/share/7dc71458-ee07-43db-9327-96d0a4fa2ae2). 

## Limitations
There may be missed opportunites from the following:
- The sentiment model is trained on twitter data instead of reddit data. As there could be intricacies in the writing style and words used, the outcome could be more acccurate given a model trained on crypto reddit data. 
- ChatGPT can make mistakes. If you took a close look at the suggested code for the summarizer, you'd see that it included duplicate `temperature` attribute which threw an error. I have not taken a detailed looked into the API to understand the available attributes and how the model can be better tuned.
- Using openai API is like using a blackbox i.e. despite using the `text-davinci-003` model, I am only aware that is is a pre-trained transformer neural network model and am unclear of the mechanisms. The dataset could be copied into ChatGPT to retrieve a summary. The output will be different and may be considered more/ less accurate.
- I also did not do any data cleaning to remove punctuations etc.. This may affect the accuracy of the output.
