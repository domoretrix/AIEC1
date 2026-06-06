# Session 1: Dense Vector Retrieval

### [Quicklinks]()


| 📰 Module Sheet                                                                  | ⏺️ Recording                                                                                                                                           | 🖼️ Slides                                             | 👨‍💻 Repo    | 📝 Homework                                                 | 📁 Feedback                                         |
| -------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------ | ------------------------------------------------------ | ------------- | ----------------------------------------------------------- | --------------------------------------------------- |
| [Dense Vector Retrieval](../00_Docs/Modules/01_Dense_Vector_Retrieval/README.md) | [Recording!](https://us02web.zoom.us/rec/share/sHWvo0Nd1aI0SEhKecOLEX9kFGVJJAdYfsKiuTmm8t85W48Z2lnjpnzTy8jAd8R5.PwuqibGwAZhvDd8c) passcode: `C62n^@Q!` | [Session 1 Slides](https://canva.link/htfqf8i39yejyhn) | You are here! | [Session 1 Assignment](https://forms.gle/Z9qskfVaAvPjn6gz8) | [Feedback 6/2](https://forms.gle/21a2uoL9DVZPwgJP6) |


## 🏗️ How AIM Does Assignments

> 📅 **Assignments will always be released to students as live class begins.** We will never release assignments early.

Each assignment will have a few of the following categories of exercises:

- ❓ **Questions** - these will be questions that you will be expected to gather the answer to. These can appear as general questions, or questions meant to spark a discussion in your breakout rooms.
- 🏗️ **Activities** - these will be work or coding activities meant to reinforce specific concepts or theory components.
- 🚧 **Advanced Builds (optional)** - Take on a challenge. These builds require you to create something with minimal guidance outside of the documentation.

## Main Assignment

In this assignment, you will build a vector RAG application using LangChain v1, OpenAI embeddings, and Qdrant.

The main notebook is:

```text
01_Cat_Health_Vector_RAG_LangChain_Qdrant.ipynb
```

The notebook uses the bundled cat health guideline PDF in `data/cat_health_guidelines.pdf`.

### Setup

From this folder, install the environment with uv:

```bash
uv sync
```

Then open the notebook in Cursor or VS Code and select the Python/Jupyter environment created by uv.

You will also need an OpenAI API key available when running the notebook.

---

## 🏗️ Activity #1: Embedding Similarity

Run the embedding similarity primer in the notebook.

You will compare embeddings for terms like:

- `king`
- `queen`
- `banana`
- `cat`
- `veterinarian`
- `cat health guidelines`

#### ❓Question #1

Why is cosine similarity useful for dense vector retrieval?

##### ✅ Answer: Cosine similarity is useful for dense vector retrieval because it computes how close are 2 vectors in a hyperdimensional space. Hence, you can use it to compare a given prompt embedding against all possible candidates embeddings and retrieve the result(s) with the highest meaning (highest cosine similarity).

---

## 🏗️ Activity #2: Build the Vector RAG Pipeline

Run the notebook sections that:

1. Load the PDF into LangChain `Document` objects
2. Split the document into chunks
3. Embed the chunks
4. Store the chunk embeddings in in-memory Qdrant
5. Retrieve relevant chunks with similarity scores
6. Generate an answer grounded in retrieved context

#### ❓Question #2

Why is metadata important for a RAG application?

##### ✅ Answer: The metadata contains the human readable text chunk that is embedded. Upon retrieval you can reference this text and bring it in your reasoning context.

##### You can also use metadata to improve search resolution by storing a key like subject or topic, to filter documents prior to cosine similarity computation, and therefore reducing cost and increasing search speed.

#### ❓Question #3

What tradeoff do we make when choosing chunk size and chunk overlap?

##### ✅ Answer:

##### A higher chunk size will lead to a higher generalization of the text embedded as more information will have to represented in a similar dimensionality vector as smaller prompt embedding vector would, hence we'd lose some precision in retrieval. A lower chunk size however has the risk of missing out on context that may or may not end up as meaningful for our problem.

##### A higher chunk overlap will allow for more granular disections of the text. At the same time will create lots of redundancies in the retrieved text because the overlapping text will be brought over more than once.

#### ❓Question #4

What does a similarity score help you understand, and what does it not prove by itself?

##### ✅ Answer: A similarity score helps you understand how close in a hyperdimensional space are the representational embeddings of 2 words or chunks of text, and in theory how close these 2 words are in meaning. The similarity score by itself however, does not prove absolute contextual relevance and therefore it would need to be compared against all carthesian product (prompt * chunks) resulting scores to retrieve the closest meaning.

---

## 🏗️ Activity #3: Vibe Check Retrieval Quality

Run the notebook's vibe check queries and inspect both:

- The retrieved context
- The generated answer

#### ❓Question #5

For the vibe check queries, did the retrieved context seem relevant before generation? Why or why not?

##### ✅ Answer: The first few sources are quite relavant, at least the first question, where the question is relevant to the data pdf it's answering from. However, in cases, where the prompt is irrelevan to the content in the data we're querying from, like the taxes question, even the first retrieved sources are not relevant.

---

## 🏗️ Activity #4: Tune Retrieval

Improve retrieval quality by changing one or more of:

- Chunk size
- Chunk overlap
- Retrieval `k`
- Query wording

Document what changed and whether retrieval improved.

##### Settings Changed:

Carthesian product combination of:

- Chunk size
- Chunk overlap
- Retrieval `k`

##### Results:

##### Chunk size seems to be the most impactful setting over the quality of the answer. In any combination with low/high overlap and low/high k value, the marginal difference is small. A small chunk size brings poor answer results in any combination of overlap and k value.

##### k is the next most impactful setting when the chunk size is medium and above. A lower k will narrow down the context and the answer is less verbose and straight to the point. On the other hand, a high k value will create a more verbose answer.

##### The overlap makes the most impact when the chunk size is medium to high and the k value is medium and low. In this case, selecting a higher overlap improves the answer by bringing in more relevant chunk selections as the same text is dissected in higher granularities.

##### Overall, I find the quality of the retrieved chunks to be the best when the chunk size is at least medium (1000) with at least a 20% overlap and a smaller k value (in this case 1-5).

---

## Optional Deep Dive: RAG From Scratch

If you want to look underneath the library abstractions, run the optional reference notebook:

```text
02_Cat_Health_Vector_RAG_From_Scratch.ipynb
```

It builds the same retrieval pipeline again with only:

- `pypdf` for extracting text from the PDF
- Python standard-library HTTP requests for calling OpenAI
- Handcrafted document, chunking, embedding, similarity-search, vector-store, and generation primitives

This notebook is a reference walkthrough, not an additional assignment. Its purpose is to make the responsibilities hidden by LangChain, Qdrant, and provider SDKs visible.

---

## Submitting Your Homework

### Main Assignment

Follow these steps to prepare and submit your homework:

1. Pull the latest updates from upstream into the main branch of your AIE9 repo:

```bash
git checkout main
git pull upstream main
git push origin main
```

1. Start Cursor from the `01_Dense_Vector_Retrieval` folder.
2. Complete the notebook.
3. Answer the questions in this `README.md`.
4. Add, commit, and push your modified work to your origin repository.

When submitting your homework, provide the GitHub URL to your AIE9 repo.