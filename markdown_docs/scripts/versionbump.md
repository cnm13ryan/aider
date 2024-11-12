## FunctionDef check_cog_pyproject
**check_cog_pyproject**: The function of check_cog_pyproject is to ensure that the project's pyproject.toml file is valid using the cog tool.
**parameters**: This function does not take any parameters.
**Code Description**: The `check_cog_pyproject` function runs the `cog --check pyproject.toml` command to validate the configuration in the `pyproject.toml` file. If this command fails (i.e., returns a non-zero exit code), it prints an error message, re-runs the cog tool with the `-r` flag to resolve issues, and then exits the program with a status code of 1. This function is called before other checks in the `main` function to ensure that the project's configuration is correct before proceeding with version bumping operations.
- The function starts by running `cog --check pyproject.toml`, capturing its output and return code.
- If the command fails (return code is non-zero), it prints an error message indicating that there are issues in the `pyproject.toml` file.
- It then re-runs the cog tool with the `-r` flag to attempt to resolve any detected issues.
- After attempting to fix the configuration, if the second run still fails, the function exits the program with a status code of 1.

**Note**: This function ensures that the project's `pyproject.toml` file is properly configured before proceeding with version bumping operations. It helps in preventing potential errors or issues during the version update process by validating the configuration upfront.
**Output Example**: If there are issues in the `pyproject.toml` file, the output might look like:
```
Error: Issues found in pyproject.toml. Please correct and run again.
```
## FunctionDef main
### Object: CustomerPayment

**Overview**

The `CustomerPayment` object is designed to manage financial transactions related to customers within our system. This object plays a crucial role in tracking payments made by customers and ensuring accurate record-keeping.

**Fields**

1. **paymentId (String)**
   - **Description:** A unique identifier for the payment transaction.
   - **Purpose:** To uniquely identify each payment entry.

2. **customerId (String)**
   - **Description:** The ID of the customer associated with this payment.
   - **Purpose:** To link payments to specific customers and manage their financial records effectively.

3. **paymentDate (DateTime)**
   - **Description:** The date and time when the payment was made.
   - **Purpose:** To record the exact timestamp of each payment transaction for audit purposes.

4. **amount (Decimal)**
   - **Description:** The monetary value of the payment.
   - **Purpose:** To accurately capture the amount paid by the customer, ensuring financial accuracy in records.

5. **paymentMethod (String)**
   - **Description:** The method used to make the payment (e.g., credit card, bank transfer).
   - **Purpose:** To provide context and traceability for each payment transaction.

6. **status (Enum: [Pending, Processed, Failed])**
   - **Description:** The current status of the payment.
   - **Purpose:** To indicate whether the payment has been successfully processed or is pending further action.

7. **description (String)**
   - **Description:** A brief description of the payment details or any relevant notes.
   - **Purpose:** To provide additional context and information for each payment transaction, aiding in quick reference.

**Usage**

The `CustomerPayment` object is primarily used to store and manage payment records within our system. It is essential for generating financial reports, tracking customer payments, and ensuring accurate billing processes.

**Example Usage:**

```python
# Creating a new CustomerPayment record
new_payment = {
    "paymentId": "PMT-001",
    "customerId": "CUS-007",
    "paymentDate": "2023-10-05T14:30:00Z",
    "amount": 150.00,
    "paymentMethod": "Credit Card",
    "status": "Processed",
    "description": "Payment for October rent"
}

# Inserting the record into the database
customer_payments.insert(new_payment)
```

**Best Practices**

- Ensure that all fields are populated accurately to maintain data integrity.
- Regularly validate and update payment statuses based on transaction outcomes.
- Use consistent naming conventions and field values to facilitate easier data analysis.

By adhering to these guidelines, you can effectively manage customer payments and ensure accurate financial records within the system.
### FunctionDef check_branch
**check_branch**: The function of check_branch is to verify that the current Git branch is main before proceeding.
**parameters**: This function does not take any parameters.

**Code Description**: 
The `check_branch` function ensures that the script is being run on the correct Git branch, specifically the "main" branch. Here's a detailed breakdown:

1. **Subprocess Execution**: The function uses Python’s `subprocess.run()` to execute a shell command: `git rev-parse --abbrev-ref HEAD`. This command retrieves the current branch name in a concise format (e.g., "main", "feature/xyz").

2. **Output Capture and Cleaning**: 
   - `capture_output=True` ensures that both stdout and stderr are captured.
   - `text=True` specifies that the output should be returned as text rather than bytes.
   - `.stdout.strip()` removes any leading or trailing whitespace from the branch name.

3. **Branch Validation**:
   - The function compares the retrieved branch name with "main".
   - If the current branch is not "main", it prints an error message: "Error: Not on the main branch." and then exits with a non-zero status code (1) using `sys.exit(1)`.

4. **Error Handling**:
   - The function handles potential errors gracefully by ensuring that only the "main" branch is allowed to proceed.
   - If any unexpected issues occur during subprocess execution, such as if Git is not installed or accessible, these would be implicitly handled within the `subprocess.run()` call and could lead to an error state.

**Note**: Ensure that the script is run from a directory where Git commands are executable. Additionally, consider adding logging or more detailed error messages for debugging purposes in production environments.
***
### FunctionDef check_working_directory_clean
**check_working_directory_clean**: The function of check_working_directory_clean is to ensure that the working directory is clean before proceeding with version bump operations.
**parameters**: This Function does not take any parameters.

**Code Description**: 
The `check_working_directory_clean` function performs a critical task in ensuring that the current working directory is free from uncommitted changes or conflicts, which are essential for maintaining the integrity of version control operations. Here’s a detailed analysis:

1. **Command Execution**: The function uses the `subprocess.run` method to execute the Git command `"git status --porcelain"`. This command provides a concise view of the state of the working directory by listing files that have been modified, staged for commit, or untracked.
   
2. **Output Capture**: The `capture_output=True` and `text=True` arguments are used to capture both the standard output and error streams as text. This allows the function to read the Git status output directly into a string variable named `status`.

3. **Condition Check**: If the `status` string is not empty, it indicates that there are uncommitted changes or other issues in the working directory. In such cases, the function prints an error message: "Error: Working directory is not clean." This informs the user that the operation cannot proceed due to a dirty working directory.

4. **Exit with Error**: The `sys.exit(1)` call terminates the program execution with a non-zero exit code (1), which typically signifies an error condition in Unix-like systems. This ensures that any dependent scripts or processes will recognize that something went wrong and can handle it appropriately.

**Note**: Before running version bump operations, ensure that the working directory is clean to avoid potential issues such as committing untested changes or losing work due to unexpected conflicts.
***
### FunctionDef check_main_branch_up_to_date
**check_main_branch_up_to_date**: The function of check_main_branch_up_to_date is to ensure that the local main branch is up-to-date with the origin/main branch.

**parameters**: This function does not take any parameters.
- None

**Code Description**: 
1. **Fetching Remote Changes**: The function starts by running `git fetch origin` using subprocess.run, which ensures that the local repository has all the latest changes from the remote repository (origin). The check=True parameter ensures that an exception is raised if there are any issues fetching the changes.
2. **Identifying Local Main Branch Commit Hash**: It then retrieves the commit hash of the local main branch by running `git rev-parse main`. This command returns the SHA-1 hash of the specified ref, in this case, "main". The output is stripped and stored as `local_main`.
3. **Printing Local Main Branch Commit Hash**: The function prints a message indicating the commit hash of the local main branch.
4. **Identifying Remote Main Branch Commit Hash**: Similarly, it retrieves the commit hash of the remote origin/main branch using `git rev-parse origin/main`. This output is also stripped and stored as `origin_main`.
5. **Printing Remote Main Branch Commit Hash**: The function prints a message indicating the commit hash of the remote main branch.
6. **Comparing Local and Remote Commit Dates**: If the local and remote commit hashes do not match, it proceeds to compare their dates using the `git show -s --format=%ci` command, which shows the date when the commit was made in ISO 8601 format (e.g., "2023-10-05 14:30:00 +0200").
7. **Date Parsing and Comparison**: The dates are parsed into datetime objects for comparison. If the local date is earlier than the remote date, it prints an error message indicating that the local main branch is behind origin/main and suggests pulling the latest changes.
8. **Handling Different Scenarios**:
   - If the local date is later than the remote date, it prints an error message suggesting pushing the changes to the remote repository because the origin/main branch is behind the local main branch.
   - If both dates are the same, it indicates that there has been a divergence between the branches and suggests checking for other issues.

**Note**: 
- Ensure that your environment has `git` installed and accessible via the command line.
- The function assumes that the current working directory is within a Git repository with a main branch.
- If any of the subprocess commands fail, an exception will be raised, causing the program to exit.
***
