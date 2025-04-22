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