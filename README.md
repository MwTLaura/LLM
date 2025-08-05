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

## Training and Performance

Although we didn’t train any new models from scratch, our project **uses a combination of two powerful pre-trained tools**:

1. **Sentence Transformers** (MiniLM model) to mathematically compare how similar texts are
2. **GPT-3.5 Turbo** to pick and justify the best recommendation from a shortlist

To evaluate whether the recommendations made sense given the user’s preferences, we used a method called **cosine similarity**, a common metric in Natural Language Processing. Here’s how it works:

> **What is Cosine Similarity?**
> Imagine every sentence (like a book synopsis or a user’s mood) as a point in a high-dimensional space. Cosine similarity measures how closely two points "point in the same direction."

> A score of 1.0 means they’re exactly the same, 0.0 means indicates orthogonality (no similarity), and -1 indicates vectors pointing in opposite directions.

> This lets us ask: *"Does this summary sound like what the user is asking for?"*

We computed **two types of similarity scores**:

* Between the **user prompt** and the **synopsis** (Does the content align?)
* Between the **user prompt** and the **justification** GPT gave (Does the reasoning match expectations?)

---

### Anime Recommendation: *Sentimental Journey*

* **User prompt:**
  *"I'm in the mood for something dramatic and psychological with a strong female lead"*

* **GPT Response:**
  Recommends *Sentimental Journey*, an anime exploring the emotional lives of 12 different girls, each with a deeply personal story. It highlights themes like love, loss, and psychological introspection—matching the mood of the user prompt.

* **Performance Metrics:**

  * **Synopsis Similarity:** 0.415
  * **Justification Similarity:** 0.521

**Interpretation:**
The model found a moderately relevant synopsis, but the real alignment came through GPT’s reasoning. This suggests that even if a summary isn’t a perfect match, GPT’s reasoning can bridge the gap between raw content and user intent.


### Book Recommendation: *The Shadow of the Wind*

* **User prompt:**
  *"I want to read something mysterious and thought-provoking with deep character development"*

* **GPT Response:**
  Suggests *The Shadow of the Wind*, a postwar mystery that follows a boy unraveling the secrets of a forgotten author. The book offers a richly detailed plot with atmospheric suspense and emotionally complex characters.

* **Performance Metrics:**

  * **Synopsis Similarity:** 0.503
  * **Justification Similarity:** 0.598

**Interpretation:**
Here, both the synopsis and GPT's justification scored quite high, showing strong alignment with the user’s intent. This is an example of a case where the recommender and LLM both worked smoothly together.


### Movie Recommendation: *The Dark Below*

* **User prompt:**
  *"I'm in the mood for a suspenseful thriller with emotional depth and a dark atmosphere"*

* **GPT Response:**
  Picked *The Dark Below*, an experimental thriller set in a haunting location. The justification emphasized the film's emotional weight, unique pacing, and eerie tone—all resonating with the request.

* **Performance Metrics:**

  * **Synopsis Similarity:** 0.515
  * **Justification Similarity:** 0.707

**Interpretation:**
Even though the movie’s official synopsis was vague and minimal, GPT still made a strong case for it. This highlights the value of using both cosine similarity **and** GPT’s interpretive capabilities to create a well-rounded recommender system.

---

## Conclusions

This project demonstrated how pre-trained Large Language Models (LLMs) can be combined with embedding-based similarity scoring to create a zero-shot recommendation system across different media domains: **anime**, **books**, and **movies**.

We used the **Sentence-BERT MiniLM model** to numerically evaluate how closely user prompts align with available media descriptions (synopses/overviews), and then passed the top matches to **GPT-3.5 Turbo** to interpret and recommend one item, complete with a justification.

Key takeaways:

* Even without fine-tuning, this hybrid system could capture abstract user moods like *“dark atmosphere,” “strong female lead,” or “deep character development.”*
* **Cosine similarity** was useful for narrowing down large datasets to a shortlist of highly relevant candidates.
* GPT’s reasoning often clarified or strengthened the connection between user intent and selected items, especially when metadata was sparse or vague.
* This method scales across domains (anime, books, movies) with little effort, proving its flexibility and generalization power.

---

## Future Work

There are several interesting directions to expand this project:

* **Web scraping for dynamic libraries**: Instead of static Kaggle datasets, we could scrape the latest titles from sources like MyAnimeList, Goodreads, and IMDb.
* **Multimodal recommendations**: Incorporate visuals (e.g., posters or trailers) into embeddings to enrich recommendations.
* **Interactive app**: Deploy the system into a Streamlit or Gradio interface so users can type their mood and instantly get recommendations.
* **Ranking multiple results**: Extend GPT’s task to return a ranked list of top 3–5 items with pros and cons for each.
* **Evaluation with human feedback**: Run user studies or use crowd-sourced validation to compare GPT’s picks with real preferences.
* **Use larger models**: Explore whether GPT-4 or Claude-3 improves recommendation quality and reasoning depth.

---

## How to Reproduce

1. Clone the Repository
2. Install Required Packages:`openai`, `sentence-transformers`,`scikit-learn`, `pandas`, `tf-keras`
3. Prepare the Datasets: 

Download the datasets from the following sources:

* 📚 [Books Dataset](https://www.kaggle.com/datasets/abdallahwagih/books-dataset)
* 📺 [Anime Dataset](https://www.kaggle.com/datasets/tanishksharma9905/top-popular-anime)
* 🎥 [Movies Dataset](https://www.kaggle.com/datasets/rounakbanik/the-movies-dataset)

Place the CSV files in a `/data/` folder or update the file paths in the notebook.

4. Run the entire Notebook 'LLMProject.ipynb'

Ensure you have an OpenAI API key:

openai.api_key = "My_API_Key" (Replace with your own API key)


---

## Citations

* Reimers, Nils, and Iryna Gurevych. ["Sentence-BERT: Sentence Embeddings using Siamese BERT-Networks."](https://arxiv.org/abs/1908.10084) arXiv preprint arXiv:1908.10084 (2019).
* OpenAI. ["GPT-3.5 Turbo."](https://platform.openai.com/docs/models/gpt-3-5)
* [Books Dataset – Kaggle](https://www.kaggle.com/datasets/abdallahwagih/books-dataset)
* [Anime Dataset – Kaggle](https://www.kaggle.com/datasets/tanishksharma9905/top-popular-anime)
* [Movies Dataset – Kaggle](https://www.kaggle.com/datasets/rounakbanik/the-movies-dataset)




