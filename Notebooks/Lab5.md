# Lab 5: End-to-End Fair Loan Approval AI Agent

## Description

This notebook implements a thesis-based AI system. This system acts as a fairness-aware loan approval decision-making agent. The core objective is to ensure fairness by actively preventing decisions from being influenced by sensitive attributes (like gender, marital status, education level), as designed in Lab 1.4.

> **Note**: This implementation is adapted to use a specific API client syntax (`genai.Client`) and model name (`gemini-2.5-flash`) to resolve a persistent environment and model availability issue.

## View the Full Implementation

To see the complete Python code, API setup, and live execution, please view the Google Colab Notebook:

🔗 [Google Colab Notebook](https://colab.research.google.com/drive/124iXS5eHkZugUVQbCRd7yvA26VHHnwEW#scrollTo=w8RZRyIXLswz)

## 1. Environment Setup

The notebook's initial phase handles all requisite configurations. It installs the `google-generativeai` library, imports all packages using the specific `from google import genai` syntax (verified as functional within our environment), securely loads the `GEMINI_KEY` from Colab's key store, and initialises a global API client.

## 2. Prompt Engineering Demonstration

This section recreates the two key prompts from Lab 1.4. We call the API using the `client.models.generate_content()` method to demonstrate the AI's reasoning.

### Prompt 1: Zero-Shot Inference

This prompt asks the model to perform the task directly without any prior examples. It tests the model's baseline ability to understand and apply the fairness criteria from scratch.

### Prompt 2: Few-Shot Inference

This prompt is more sophisticated. It provides the model with known examples (X and Y) before asking it to evaluate new cases. This "primes" the model on the correct logic (e.g., "Credit History: 0" leads to "Rejected"), resulting in more consistent and accurate predictions.

## 3. End-to-End System Demonstration

This section transforms the designed prompt into a functional Python pipeline, utilising the client object and model name initialised during the setup phase.

A function named `run_fair_loan_pipeline` is defined, simulating how the AI system processes new loan applications from raw data to ultimately reaching a fair decision.

### Process Flow:

1. **Data Understanding (Input)**: A new applicant's information is received as a Python dictionary.
2. **Reasoning**: The new data is formatted and injected into our core reasoning logic—the few-shot prompt (Prompt 2) template.
3. **Inference**: The completed prompt is sent to the Gemini API using `client.models.generate_content()`.
4. **Output Generation**: Gemini processes the prompt and returns the decision (Y) and the justification, which are then printed.

The notebook concludes by running this pipeline with a new test applicant to demonstrate the end-to-end system in action.

## 4. Reflection

### Functionality of the system as a complete AI agent:

Our system is an AI agent specifically designed for loan application assessment, with its primary objective being to ensure fairness. It receives applicant data (X), applies predefined fairness rules, and generates an approval/rejection decision (Y) along with corresponding explanations. This process ensures decisions are based solely on valid financial factors.

### Application of Gemini and prompt engineering:

Gemini and prompt engineering form the core of this system. In this experiment, we employ Gemini as the 'reasoning engine'. Prompt engineering is used to "program" its behaviour. Specifically, we achieve 'attribute exclusion' or 'fairness through unconsciousness' by explicitly instructing Gemini to disregard fields such as gender, marital status, and educational attainment. Sample-based prompts prove most effective, as they provide concrete examples that help the model reliably learn the required logic (e.g., poor credit history leads to rejection).

Gemini's strength lies in its ability to simultaneously adhere to negative constraints (instructions to omit certain data) and example-based logic (few-shot reasoning). The system is remarkably straightforward to implement and interpret—excluding sensitive attributes constitutes a clear and auditable strategy.

### Room for improvement lies in the complexity of fairness standards themselves:

'Fairness through ignorance' is a foundational approach that cannot account for proxy variables (e.g., bias may still arise if 'property location' is highly correlated with a sensitive attribute). More advanced systems may require exploring post-processing techniques or more sophisticated fairness metrics beyond simple input exclusion.
