# LLM-BASED MEDIA RECOMMENDER SYSTEM

![UTA-DataScience-Logo](https://github.com/user-attachments/assets/fec1b411-bda5-437a-9eb8-08a018eb84ae)

## **One Sentence Summary**
This repository explores the use of Large Language Models (LLMs) like gpt-3.5-turbo to recommend books, movies, and anime based on natural language prompts, using cleaned Kaggle datasets for each media domain.

## **Overview**
This project investigates how pre-trained LLMs can function as zero-shot recommendation systems for various types of media: books, anime, and movies. Unlike traditional recommenders that rely on user-item interaction data (e.g., ratings or viewing history), this approach allows users to describe what they’re in the mood for in free-form natural language—such as “a suspenseful thriller with emotional depth and a dark atmosphere.”

The pipeline consists of:

- Embedding a user prompt using SentenceTransformers (all-MiniLM-L6-v2)

- Computing cosine similarity between that prompt and item descriptions (synopses) from Kaggle datasets

- Shortlisting the top 15 matches for each domain

- Passing that shortlist to an LLM (gpt-3.5-turbo) to select the most suitable title and explain the recommendation

This approach allows us to test whether an LLM can generalize across media types using only high-level context and descriptions, without retraining or explicit preference data.

## **Performance Summary**
Since the LLM is used in a zero-shot setting, we evaluate the quality of its recommendations using cosine similarity metrics. Specifically, we compute:

- Cosine similarity between the user prompt and the recommended title’s synopsis

- Cosine similarity between the user prompt and the LLM’s justification

This gives us a way to numerically assess semantic alignment between the user's intent and the recommendation.

In our experiments:

- Cosine scores between the prompt and final recommendations typically ranged between 0.37 and 0.7, and varies depending on the prompt

- GPT’s explanations were coherent and aligned with user prompts in all three media domains

- The method successfully avoided irrelevant suggestions by filtering with embedding similarity prior to LLM interaction

---


