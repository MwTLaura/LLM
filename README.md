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

Great! Here's the next section, broken into all relevant parts for **Summary of Work Done**:

---

## Summary of Work Done

### Data

We used publicly available Kaggle datasets across three media categories:

- **Books:** [Books Dataset](https://www.kaggle.com/datasets/abdallahwagih/books-dataset)
- **Anime:** [Top Popular Anime](https://www.kaggle.com/datasets/tanishksharma9905/top-popular-anime)
- **Movies:** [The Movies Dataset](https://www.kaggle.com/datasets/rounakbanik/the-movies-dataset?select=movies_metadata.csv)

Each dataset included metadata such as title, genres, synopsis/overview, and popularity or score metrics.

* **Instances:**

  * Anime: \~28,000 entries
  * Books: \~8,000 entries
  * Movies: \~45,000 entries

### Preprocessing / Clean-Up

- Filtered out missing or empty descriptions
- Retained key columns: `Title`, `Genres`, and `Synopsis/Description`
- For each domain, created a concise dataframe (e.g., `anime_df`, `books_df`, `movies_df`) with clean entries ready for semantic embedding and prompting

No heavy preprocessing or feature engineering was needed.

---

### Problem Formulation

#### **Task**

Build a zero-shot media recommender system that accepts natural language preferences and suggests an appropriate anime, book, or movie title with reasoning.

#### **Input / Output**

- **Input:**
  Natural language user prompt (e.g., “I want a movive/book/anime that is sad and introspective about grief and healing”)

- **Output:**
  A single recommended item with:

  * Title
  * Genres
  * Synopsis
  * GPT-generated justification

#### **Models & Tools**

* **Embedding Model:** [`all-MiniLM-L6-v2`](https://huggingface.co/sentence-transformers/all-MiniLM-L6-v2)
  Used to create vector embeddings for user prompt and each media synopsis

* **LLM:** OpenAI `gpt-3.5-turbo`
  Used to select and explain a single best match from a top-15 shortlist

---


