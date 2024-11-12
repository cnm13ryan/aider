## FunctionDef is_important(file_path)
**is_important**: The function of is_important is to determine if a given file path represents an important file within a project.

**parameters**:
· parameter1: `file_path` (str) - A string representing the full path to the file being checked.

**Code Description**:
The function `is_important` checks whether a given file path is considered important based on specific criteria. It first extracts the base name and directory of the provided file path using `os.path.basename` and `os.path.dirname`. The directory is then normalized with `os.path.normpath` to ensure consistent comparisons.

The function uses two main conditions to determine if the file is important:
1. **GitHub Actions Workflow Files**: If the directory name matches `.github/workflows` (also normalized) and the base filename ends with ".yml", the function returns `True`.
2. **Normalization Check**: The function also checks if the normalized path of the provided file exists in a predefined list of important files, `NORMALIZED_ROOT_IMPORTANT_FILES`.

The callers of this function use it to filter out non-important files from a list of potential candidates. For example, `filter_important_files` utilizes `is_important` to return only those paths that are considered significant.

**Note**:
- Ensure that the list `NORMALIZED_ROOT_IMPORTANT_FILES` is correctly defined and includes all necessary important file paths.
- The function assumes that the input `file_path` is a valid string representing an actual file path in the project directory structure.

**Output Example**: 
If `file_path = ".github/workflows/test.yml"`, the function returns `True`. If `file_path = "src/main.py"`, it returns `False`.

This function plays a crucial role in identifying files that are likely to be important for the project, such as configuration files or setup scripts, particularly those related to GitHub Actions.
## FunctionDef filter_important_files(file_paths)
**filter_important_files**: The function of filter_important_files is to return only those file paths that are considered important within a project.
· parameter1: `file_paths` (list) - A list of file paths to check.

**Code Description**: 
The function `filter_important_files` utilizes the `is_important` function to filter out non-important files from a given list of potential candidates. Here's a detailed breakdown:

- The function takes a single parameter, `file_paths`, which is expected to be a list of file paths.
- It uses Python’s built-in `filter()` function along with the `is_important` function to iterate over each file path in `file_paths`.
- For each file path, it calls `is_important(file_path)` to determine if the file should be considered important. If `is_important` returns `True`, the file path is included in the result.
- The filtered list of important files is then returned as a new list.

The `is_important` function plays a crucial role here by checking specific conditions:
1. **GitHub Actions Workflow Files**: It checks if the directory name matches `.github/workflows` (normalized) and if the file ends with ".yml".
2. **Cache Files and Metadata**: It also considers common files like `.gitignore` and `requirements.txt`.

The `filter_important_files` function is called within the `test_filter_important_files` test case to ensure that only important files are returned, as demonstrated by the expected output.

**Note**: Ensure that all file paths provided in `file_paths` are correctly formatted and exist. The function does not handle cases where the input list is empty or contains invalid paths.

**Output Example**: 
Given the input list of file paths:
```python
files = [
    "README.md",
    "src/main.py",
    ".gitignore",
    "tests/test_app.py",
    "requirements.txt",
    ".github/workflows/test.yml",
    "random_file.txt"
]
```
The function will return a filtered list containing only the important files:
```python
important_files = ["README.md", ".gitignore", "requirements.txt", ".github/workflows/test.yml"]
```
