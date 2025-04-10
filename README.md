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