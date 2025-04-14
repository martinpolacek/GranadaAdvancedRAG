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

## Evaluation Results

| Model         | Average Accuracy (%) |
|---------------|-----------------------|
| Flash 2.0     | 94.237               |

*Note: Additional rows can be added for other models as evaluations are completed.*