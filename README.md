# GranadaAdvancedRAG

## Notebook Descriptions

### 1_find_valid_projects.ipynb
This notebook scans the `pages` folder to identify valid project folders. It:
- Counts the number of folders and identifies empty ones.
- Detects duplicate PDF files across folders.
- Finds non-empty folders containing unique PDFs.
- Saves the names of valid project folders to `valid_project.txt`.

### 2_generate_test_data_for_valid_projects.ipynb
This notebook generates test data for valid projects. It:
- Loads the list of valid projects from `valid_project.txt`.
- Combines text from all PDFs in each project folder.
- Uses a generative AI model to create questions and answers based on the combined text.
- Saves the generated questions and answers to the `test_data` folder.

### 3_validate_answers_per_project_full_context.ipynb
This notebook validates the answers generated for each project. It:
- Loads the list of valid projects from `valid_project.txt`.
- Reads the combined text and questions/answers for each project.
- Uses a generative AI model to validate the answers by comparing them with the original answers.
- Calculates the accuracy for each project and saves the validation results to the `validation_single` folder.

### 4_generate_summaries.ipynb
This notebook generates summaries for each valid project. It:
- Loads the list of valid projects from `valid_project.txt`.
- For each project, reads the combined text from its PDF files.
- Uses a generative AI model (Gemini 1.5 Flash) to create comprehensive summaries that include:
  - WHO: The organizers and eligible applicants/target groups
  - WHAT: The type of program/call and its focus
  - HOW: Application procedures and key requirements
  - WHY: Project objectives and expected results 
  - HOW MUCH: Financial information including support amount and co-financing rate
  - OTHER IMPORTANT INFORMATION: Deadlines, conditions, and other essential details
- Saves the generated summaries to the `summary` folder.
- Implements rate limiting (15 requests per minute) to avoid API limits.

### 5_evaluate_answers_from_summaries.ipynb
This notebook evaluates the accuracy of answers generated using only the summaries instead of the full document text. It:
- Loads the list of valid projects from `valid_project.txt`.
- Reads the summaries (instead of the combined text) and the questions/answers for each project.
- Uses the same generative AI model to answer questions based only on the summary content.
- Calculates the accuracy for each project and saves the validation results to the `validation_summary` folder.
- Compares the performance between using full text and using only summaries.

### 6_validate_answers_per_project_rag.ipynb
This notebook validates answers using a Retrieval-Augmented Generation (RAG) approach:
- Loads the list of valid projects from `valid_project.txt`.
- For each project, reads the combined text and questions/answers.
- Splits the combined text into chunks and vectorizes them using a multilingual embedding model.
- For each question, finds the most relevant chunks (1, 5, or 10 per question) based on similarity.
- Uses only these relevant chunks as context for answer generation with a generative AI model.
- Compares generated answers with the original ones and calculates accuracy.
- Saves validation results to the `validation_rag` folder.
- Allows comparison of RAG performance for different numbers of chunks per question.

### 7_validate_answers_per_project_rag t5.ipynb
This notebook is an advanced variant of the RAG validation pipeline. It:
- Loads the list of valid projects from `valid_project.txt`.
- For each project, reads the combined text and questions/answers.
- Splits the combined text into chunks and vectorizes them using a multilingual embedding model.
- For each question, first extracts keywords using KeyBERT and vectorizes only these keywords.
- Finds the most relevant chunks (typically 10 per question) based on similarity to the keyword vector.
- Uses only these relevant chunks as context for answer generation with a generative AI model.
- Compares generated answers with the original ones and calculates accuracy.
- Saves validation results to the `validation_rag` folder.
- Allows comparison of RAG performance for different retrieval/query strategies (e.g., full question vs. keywords).

### 8_generate_general_question_for_agents.ipynb
This notebook evaluates the relevance of project calls to a set of predefined general questions using summaries. It:
- Defines a list of 12 general questions applicable across various funding calls.
- Loads project summaries from the `summary` folder.
- Uses a generative AI model (Gemini 2.5 Flash) to determine, for each question, which project summaries potentially contain relevant information ("INCLUDE" vs. "EXCLUDE").
- Compares the LLM's list of "INCLUDE" calls against a baseline list (presumably generated using full text or another method) for each question.
- Calculates and prints Precision, Recall, and F1 scores for each question to evaluate the LLM's performance in identifying relevant calls based solely on summaries.
- Saves the baseline answers (from full text) to a pickle file (`answers_backup.pkl`).

## Evaluation Results

| Model                                | Average Accuracy (%) |
|--------------------------------------|-----------------------|
| Flash 2.0 (full text)                | 94.237               |
| Flash 2.0 (summaries only)           | 49.660               |
| Flash 2.0 (full text, 1 chunk per question) | 65.080               |
| Flash 2.0 (full text, 5 chunks per question) | 84.237               |
| Flash 2.0 (full text, 10 chunks per question) | 88.310               |
| Flash 2.0 (keywords query, 10 chunks per question)  | 83.900               |

*Note: Additional rows can be added for other models as evaluations are completed.*

### Evaluation of Call Relevance Identification (Gemini 2.5 Flash on Summaries)

| Question # | Precision | Recall | F1 Score |
|------------|-----------|--------|----------|
| 1          | 0.62      | 0.62   | 0.62     |
| 2          | 0.71      | 0.61   | 0.66     |
| 3          | 0.83      | 0.24   | 0.37     |
| 4          | 0.43      | 0.75   | 0.55     |
| 5          | 0.54      | 0.44   | 0.48     |
| 6          | 0.68      | 0.43   | 0.53     |
| 7          | 0.71      | 0.56   | 0.62     |
| 8          | 0.80      | 0.18   | 0.30     |
| 9          | 0.68      | 0.38   | 0.49     |
| 10         | 0.33      | 0.06   | 0.10     |
| 11         | 0.71      | 0.80   | 0.75     |
| 12         | 0.73      | 0.56   | 0.64     |

*Note: These scores evaluate the LLM's ability to identify relevant project calls for each question based on summaries, compared to a baseline.*

#### Questions used for evaluation:

1. Which call is primarily targeted at small and medium-sized enterprises (SMEs)?
2. Which call requires partnership or cooperation among multiple EU member states?
3. Which call targets the culture and creative sectors (e.g., film, media, arts)?
4. Which call is designated only for non-profit organizations or NGOs?
5. Which call supports mobility of students or youth under the Erasmus programme?
6. Which call focuses on research and development of new technologies or innovations (RIA, POC)?
7. Which call is funded under a European Joint Undertaking (JU)?
8. Which call relates to humanitarian aid or civil protection?
9. Which call includes pilot validation of technology or demonstration projects?
10. Which call aims to strengthen digital infrastructure or EDIH centres?
11. Which call is open to applicants from third countries outside the EU/EEA?
12. Which call requires emphasis on environmental sustainability or clean energy?