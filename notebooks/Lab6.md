# Lab 6: Fair Lending AI Agent Based on RAG

## Objective

This project will upgrade the fair lending AI agent from Experiment 5 into a Retrieval-Augmented Generation (RAG) system. We shall create a vector database (ChromaDB) containing 1,000 samples.

## Approach

* **Retrieval (R):** To circumvent API quota limitations, we shall utilise a local sentence transformer library (from Hugging Face) to generate word embeddings.
* **Generation (G):** We shall continue employing the Gemini API (gemini-2.5-flash) to generate the final AI decision.

## 💻 View Full Implementation

To view the complete Python code, all configurations, data generation, and the live RAG workflow, consult the Google Colab Notebook:

[➡️ Click here to open the full Google Colab Notebook](https://colab.research.google.com/drive/1dIVtbis8KKkYlX7rjzYWcw0V9kpTiU7k#scrollTo=j8TgOh7wlZfW)

## 1. Installation and Setup

The first section of the Notebook handles all necessary configurations. It installs all required libraries:

* `google-generativeai`: For the Generation (G) step.
* `chromadb`: In-memory vector database.
* `sentence-transformers`: For local embedding (R) step, avoiding API quota errors.

The code then initialises the Gemini API client (for generation) and loads the local `all-MiniLM-L6-v2` model (for embedding).

## 2. Generate 1,000 (X, y) Samples

This section generates a simulated dataset comprising 1,000 (X, y) samples. A Python function `get_loan_decision` is defined, acting as a ‘fair’ banker making decisions (approve/reject) based solely on financial factors like credit history and applicant income, while ignoring sensitive attributes such as gender or educational attainment.

## 3. Storing in ChromaDB vector database

This forms the core of the retrieval (R) step. The auxiliary function `format_applicant_for_embedding` converts each applicant's raw JSON data into natural language strings. All 1000 document strings are encoded into vectors using a local sentence transformer model. This step is rapid, requires no API calls, and successfully circumvents API quota limitations. These 1000 embedding vectors, along with their corresponding metadata (original applicant JSON and fair decision y), are added to the `loan_applications` collection within ChromaDB.

## 4. Complete RAG Process Demonstration

This section demonstrates the complete end-to-end RAG system required by the laboratory:

1.  **Input:** Define a new applicant X\_new.
2.  **Retrieval:** First, embed the X\_new data using the local model.
3.  **Reinforcement:** Construct a detailed prompt. This prompt instructs the AI to assume the role of an impartial loan officer and mandates the use of the retrieved three cases as contextual reference.
4.  **Generation:** The final reinforced prompt is submitted to the Gemini API (gemini-2.5-flash), which then generates the final decision and accompanying explanation.

## 5. Lab 6 Summary (Reflection)

### How did RAG enhance my system?

Upgrading my Fair Loan Agent from the ‘few-shot’ prompt (Experiment 5) to the RAG workflow (Experiment 6) delivered a qualitative leap. RAG substantially improved the system's reliability and consistency. In Experiment 5, I had to manually select two or three ‘generic’ examples; now, the system can automatically retrieve the three most relevant cases from 1,000 instances associated with the current applicant.

This “dynamic context” renders the AI's decisions **“evidence-based”**. The model no longer merely adheres to an abstract “fairness rule”, but is compelled to reference genuine (simulated) past precedents from the database. For instance, should a new applicant with “high income, high loan amount” emerge, RAG instantly locates three cases similarly rejected for this reason, rendering it virtually impossible for the AI to erroneously approve. By integrating ‘rules’ with ‘data,’ RAG significantly reduces the likelihood of models hallucinating or deviating from core fairness logic.

## 6. Independent Database Query (Final Demonstration)

This section independently demonstrates the retrieval process. It employs a new textual query (‘applicant with poor credit history’) against the database (constructed in Cell 7) to illustrate semantic search functionality, successfully retrieving relevant entries from a database containing 1,000 records.
