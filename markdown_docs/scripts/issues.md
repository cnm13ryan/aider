## FunctionDef has_been_reopened(issue_number)
**has_been_reopened**: The function of `has_been_reopened` is to check if an issue has been reopened based on its timeline events.
**parameters**:
· parameter1: `issue_number` - An integer representing the number of the GitHub issue being checked.

**Code Description**: 
The function `has_been_reopened` takes a single argument, `issue_number`, which represents the ID of a GitHub issue. It constructs a URL to access the timeline events for that specific issue using the GitHub API. By making a GET request to this URL with appropriate headers, it retrieves the JSON response containing all events related to the issue's timeline.

The function then iterates through each event in the JSON response. If an event is found where the `event` key has the value "reopened", the function immediately returns `True`, indicating that the issue has indeed been reopened at some point. The iteration stops as soon as such an event is detected to avoid unnecessary processing.

If no events with the "reopened" action are found, the function will return `False`, meaning the issue has not been reopened according to its timeline.

This function is crucial for various operations in the project, particularly those involving issue management and filtering. For instance, it is used within the `group_issues_by_subject` function to exclude issues that have been reopened from being included in certain groups based on their titles. Similarly, it plays a role in identifying stale questions by ensuring that only issues still marked as "open" are considered for further processing.

**Note**: Ensure that the GitHub API URL and headers (including authentication) are correctly configured before calling this function to avoid errors. Also, be aware of rate limits imposed by the GitHub API to prevent overloading the server with too many requests.

**Output Example**: The function returns `True` if an "reopened" event is found in the issue's timeline; otherwise, it returns `False`. For example:
```python
print(has_been_reopened(123))  # Output: False (if no reopened events)
print(has_been_reopened(456))  # Output: True (if an "reopened" event is found)
```
## FunctionDef get_issues(state)
**get_issues**: The function of get_issues is to fetch issues from a GitHub repository based on specified state.
**parameters**:
· state: A string indicating the state of issues to be fetched ("open", "closed", or "all"). Default value is "open".

**Code Description**: 
The `get_issues` function retrieves all issues from a specific GitHub repository, filtering by state. It uses pagination to ensure that all issues are collected, regardless of their total number.

1. **Initialization and Total Count Calculation**: The function initializes an empty list `issues` to store the fetched issues. It then calculates the total count of pages required based on the response headers from the first API call with a per_page parameter set to 1.
2. **Progress Bar Setup**: A progress bar is created using `tqdm` to display the number of pages being processed, providing visual feedback during execution.
3. **Issue Fetching Loop**: The function enters a loop where it makes subsequent API calls to fetch issues by iterating through each page. For each call, it updates the total list of issues and increments the page counter until all pages are fetched or no more issues are returned.
4. **Error Handling**: Each API request is made with proper headers and parameters. If any request fails, an exception will be raised due to `response.raise_for_status()`.

The function interacts closely with the main function in `issues.py`, providing it with a comprehensive list of issues based on the specified state. This data is then used by other functions like `handle_unlabeled_issues`, `handle_stale_issues`, and others, which process and manage these issues according to specific criteria.

**Note**: Ensure that you have set the necessary environment variables such as `GITHUB_API_URL`, `REPO_OWNER`, `REPO_NAME`, and `TOKEN` before calling this function. Additionally, handle any potential exceptions that may arise from network errors or API rate limits.

**Output Example**: The output of `get_issues("all")` would be a list of dictionaries representing all issues in the repository, including their titles, states, creation times, etc., depending on the structure returned by the GitHub API.
## FunctionDef group_issues_by_subject(issues)
**group_issues_by_subject**: The function of `group_issues_by_subject` is to categorize issues based on their subjects (titles) while excluding issues that have been reopened.
**parameters**: 
· parameter1: `issues` - A list of dictionaries, where each dictionary represents an issue and contains keys such as "title" and "number".

**Code Description**: The function `group_issues_by_subject` takes a list of issues as input and groups them by their titles using a regular expression pattern to filter out specific types of issues. Here is the detailed breakdown:

1. **Initialization**: A `defaultdict(list)` named `grouped_issues` is created to store the grouped issues.
2. **Pattern Definition**: The regex pattern `r"Uncaught .+ in .+ line \d+"` is defined to match titles that indicate uncaught exceptions or errors, which are often associated with bugs or unexpected behavior.
3. **Iteration and Filtering**: For each issue in the input list:
   - It checks if the title matches the defined pattern using `re.search`.
   - It also ensures that the issue has not been reopened by calling `has_been_reopened(issue["number"])`. If the issue has been reopened, it is skipped.
4. **Grouping**: For issues that match both conditions, their titles are used as keys to group them into the `grouped_issues` dictionary.

The function ultimately returns a dictionary where each key is an issue title (subject) and the value is a list of issues with that subject.

**Note**: The function relies on another function `has_been_reopened` to determine if an issue has been reopened. This ensures that only relevant issues are grouped together, avoiding issues that have already been addressed or reopened.

**Output Example**: Suppose we have a list of issues as follows:
```python
issues = [
    {"number": 1, "title": "Uncaught TypeError in line 42"},
    {"number": 2, "title": "Uncaught SyntaxError in line 50"},
    {"number": 3, "title": "Feature request for improved logging"}
]
```
The function call `group_issues_by_subject(issues)` would return:
```python
{
    "Uncaught TypeError in line 42": [{"number": 1, "title": "Uncaught TypeError in line 42"}],
    "Uncaught SyntaxError in line 50": [{"number": 2, "title": "Uncaught SyntaxError in line 50"}]
}
```
Note that the issue with a title indicating a feature request would not be included in this grouping as it does not match the defined pattern.
## FunctionDef find_oldest_issue(subject, all_issues)
**find_oldest_issue**: The function of `find_oldest_issue` is to identify the oldest open issue with a specific subject that has not been reopened.
**parameters**:
· parameter1: `subject` - A string representing the title or label of the issue group being searched.
· parameter2: `all_issues` - A list of dictionaries, where each dictionary contains details about an individual GitHub issue (e.g., "title", "created_at", and "number").
**Code Description**: The function iterates through all issues provided in the `all_issues` list to find the oldest open issue that matches the given `subject`. It first initializes `oldest_issue` as `None` and sets `oldest_date` to the current date and time. As it processes each issue, it checks if the issue's title matches the `subject` and whether the issue has been reopened using the `has_been_reopened` function.

If both conditions are met (i.e., the issue title matches the subject and the issue is still open), the function converts the "created_at" timestamp to a datetime object. If this date is earlier than `oldest_date`, it updates `oldest_date` and sets `oldest_issue` to the current issue.

Once all issues have been processed, the function returns the oldest issue that meets the criteria or `None` if no such issue exists.
**Note**: The `has_been_reopened` function is called within this function to ensure that only open issues are considered. This helps in filtering out issues that have been reopened and might not be relevant for certain operations.

The `find_oldest_issue` function is crucial for operations involving issue management, such as identifying the longest-standing unaddressed issues based on their titles. It is called by the `handle_duplicate_issues` function to find the oldest open issue within a group of similar issues before deciding whether to comment and close duplicates.
**Output Example**: The function returns an issue dictionary if it finds one that matches the criteria; otherwise, it returns `None`. For example:
```python
oldest_issue = find_oldest_issue("bug", all_issues)
if oldest_issue:
    print(f"Oldest bug: {oldest_issue['title']} created at {oldest_issue['created_at']}")
else:
    print("No matching open issue found.")
```
## FunctionDef comment_and_close_duplicate(issue, oldest_issue)
**comment_and_close_duplicate**: The function of comment_and_close_duplicate is to post a comment on an issue indicating that it is a duplicate of another issue and then close the issue.

**parameters**:
· parameter1: `issue` - A dictionary containing information about the issue to be closed, including its number.
· parameter2: `oldest_issue` - A dictionary containing information about the oldest (and presumably primary) issue related to the current one, including its number.

**Code Description**: 
The function `comment_and_close_duplicate` is designed to handle duplicate issues by interacting with a GitHub API. It performs two main actions:
1. **Posting a Comment**: The function constructs a comment body indicating that the issue being processed is a duplicate of an older issue and posts this comment on the issue using the GitHub API.
2. **Closing the Issue**: After posting the comment, it closes the issue by setting its state to "closed" via another API call.

The function uses the `requests` library to make HTTP requests to the GitHub API. It requires headers such as authentication tokens and URL constants like `GITHUB_API_URL`, `REPO_OWNER`, and `REPO_NAME` to be defined elsewhere in the codebase.

This function is called within the `handle_duplicate_issues` function, which processes a list of issues grouped by subject to identify duplicates. When duplicates are found, `comment_and_close_duplicate` is invoked for each issue that needs to be closed, ensuring that only the oldest related issue remains open while comments are added to other duplicate issues.

**Note**: Ensure that all necessary API keys and repository details are correctly configured before calling this function. Also, handle exceptions appropriately as the function uses `raise_for_status()` to check for errors in API responses.
## FunctionDef find_unlabeled_with_paul_comments(issues)
**find_unlabeled_with_paul_comments**: The function `find_unlabeled_with_paul_comments` is designed to identify open issues that do not have any labels but contain comments from the user "paul-gauthier".

**Parameters**:
· parameter1: `issues`: A list of issue dictionaries retrieved from GitHub API, each containing details such as issue number, title, and state.

**Code Description**: 
The function iterates through a list of issues to filter out pull requests by checking for the presence of "pull_request" in the issue dictionary. For open issues without any labels (`issue["labels"]` is empty), it fetches comments using the GitHub API. If any comment from "paul-gauthier" is found, the issue is added to the `unlabeled_issues` list.

The function then returns a list of such issues that meet the criteria. This process helps in identifying specific issues for further action, such as labeling them appropriately.

**Note**: 
- Ensure that the `headers` variable is defined and contains valid authentication information for accessing GitHub API.
- The `GITHUB_API_URL`, `REPO_OWNER`, and `REPO_NAME` constants should be properly set before calling this function.

**Output Example**: 
The function returns a list of dictionaries representing issues, each containing fields like "number", "title", and "html_url". For example:
```python
[
    {'number': 123, 'title': 'Issue Title', 'html_url': 'https://github.com/owner/repo/issues/123'},
    {'number': 456, 'title': 'Another Issue', 'html_url': 'https://github.com/owner/repo/issues/456'}
]
```
## FunctionDef handle_unlabeled_issues(all_issues, auto_yes)
**handle_unlabeled_issues**: The function `handle_unlabeled_issues` is designed to identify and label issues that have comments from "paul-gauthier" but are currently unlabeled.

**Parameters**:
· parameter1: `all_issues`: A list of issue dictionaries retrieved from the GitHub API. Each dictionary contains details such as issue number, title, state, labels, and URL.
· parameter2: `auto_yes`: A boolean flag indicating whether to automatically label issues without prompting the user.

**Code Description**: 
The function begins by printing a message indicating that it is searching for unlabeled issues with comments from "paul-gauthier". It then calls another function, `find_unlabeled_with_paul_comments`, passing in the list of all issues. This function returns a filtered list of issues that meet the criteria (open issues without labels and containing comments from "paul-gauthier").

If no such issues are found, it prints a message indicating this fact and exits early. Otherwise, it prints the total number of unlabeled issues with "paul-gauthier" comments and lists each issue's number, title, and URL.

The function then checks if the `auto_yes` parameter is set to False. If so, it prompts the user for confirmation before proceeding to label the issues. The user can choose not to proceed by entering anything other than 'y' (yes).

If the user confirms or if `auto_yes` is True, the function proceeds to add the "question" label to each of the identified unlabeled issues. It constructs a URL for updating the issue labels and uses the `requests` library to send a PATCH request to the GitHub API. Upon successful labeling, it prints a confirmation message indicating that the "question" label has been added.

**Note**: 
- Ensure that the `headers` variable is defined and contains valid authentication information for accessing the GitHub API.
- The `GITHUB_API_URL`, `REPO_OWNER`, and `REPO_NAME` constants should be properly set before calling this function.

**Output Example**: 
The function will output messages like:
```
Finding unlabeled issues with paul-gauthier comments...
Found 3 unlabeled issues with paul-gauthier comments:
  - #123: Issue Title https://github.com/owner/repo/issues/123
  - #456: Another Issue https://github.com/owner/repo/issues/456
  - #789: Yet Another Issue https://github.com/owner/repo/issues/789

Do you want to add the 'question' label to these issues? (y/n): 
Adding 'question' label to issue #123...
Adding 'question' label to issue #456...
Adding 'question' label to issue #789...
```
## FunctionDef handle_stale_issues(all_issues, auto_yes)
**handle_stale_issues**: The function of handle_stale_issues is to identify and process stale question issues on GitHub.

**Parameters**:
· parameter1: `all_issues` - A list of dictionaries representing all issues from a repository.
· parameter2: `auto_yes` - A boolean flag indicating whether to automatically confirm actions without user input (default is False).

**Code Description**:  
The function `handle_stale_issues` plays a crucial role in managing stale questions on GitHub. It iterates through the list of issues provided, filtering out those that are not open, do not have the "question" label, already marked as "stale", or have been reopened. For each issue that passes these filters and has been inactive for 14 days, it prints a message indicating the issue number, title, URL, and inactivity period.

If `auto_yes` is set to False, the function prompts the user with a confirmation message asking whether to add a "stale" label and comment. If the user does not confirm by entering 'y', the function skips this particular issue. Otherwise, it proceeds to perform two actions:
1. Adds a comment using the `STALE_COMMENT` string via a POST request to the GitHub API.
2. Applies the "stale" label to the issue through a PATCH request.

The function ensures that only open issues are considered for processing and handles cases where an issue has been reopened, thereby avoiding unnecessary labeling or commenting on such issues. This process helps in maintaining the repository by keeping track of inactive questions and ensuring they receive appropriate attention.

**Note**: Ensure that the GitHub API URL and headers (including authentication) are correctly configured before calling this function to avoid errors. Also, be aware of rate limits imposed by the GitHub API to prevent overloading the server with too many requests. Additionally, make sure that the `STALE_COMMENT` string is defined elsewhere in your codebase as it is used within this function. The `has_been_reopened` function is called internally to check if an issue has been reopened, ensuring only current issues are processed.

This function is part of a broader set of functions managed by the `main` function, which handles various types of GitHub issues including duplicates and unlabeled issues. By integrating with other functions like `handle_unlabeled_issues`, `handle_stale_closing`, and `handle_duplicate_issues`, it contributes to a comprehensive issue management system for repositories.
## FunctionDef handle_stale_closing(all_issues, auto_yes)
**handle_stale_closing**: The function of handle_stale_closing is to manage issues that are marked as stale but have received new activity.
**parameters**: 
· all_issues: A list of dictionaries representing GitHub issues, each containing details such as issue number, title, state, and labels.
· auto_yes: A boolean flag indicating whether the script should automatically proceed without user confirmation.

**Code Description**: The function `handle_stale_closing` is designed to identify and manage GitHub issues that are marked as stale but have shown recent activity. It performs several steps to determine whether a stale issue should be closed or if it needs its stale label removed based on new comments.

1. **Initial Message**: 
   ```python
   print("\nChecking for issues to close or unstale...")
   ```
   This line prints an initial message indicating that the script is checking for issues that need attention.

2. **Skipping Non-Open and Non-Stale Issues**:
   ```python
   if issue["state"] != "open" or "stale" not in [label["name"] for label in issue["labels"]]:
       continue
   ```
   The function first skips any issue that is not open or does not have the "stale" label.

3. **Getting Timeline to Find When Stale Label Was Added**:
   ```python
   timeline_url = f"{GITHUB_API_URL}/repos/{REPO_OWNER}/{REPO_NAME}/issues/{issue['number']}/timeline"
   response = requests.get(timeline_url, headers=headers)
   response.raise_for_status()
   events = response.json()
   ```
   It fetches the timeline of events for the issue to find when the "stale" label was last added.

4. **Finding Recent Stale Label Addition**:
   ```python
   stale_events = [
       event
       for event in events
       if event.get("event") == "labeled" and event.get("label", {}).get("name") == "stale"
   ]
   ```
   It filters the timeline events to find the most recent addition of the "stale" label.

5. **Checking for New Comments Since Stale Label**:
   ```python
   comments_url = f"{GITHUB_API_URL}/repos/{REPO_OWNER}/{REPO_NAME}/issues/{issue['number']}/comments"
   response = requests.get(comments_url, headers=headers)
   response.raise_for_status()
   comments = response.json()

   new_comments = [
       comment
       for comment in comments
       if datetime.strptime(comment["created_at"], "%Y-%m-%dT%H:%M:%SZ") > latest_stale
   ]
   ```
   The function fetches the comments on the issue and checks if any of them were created after the "stale" label was added.

6. **Handling New Activity**:
   ```python
   if new_comments:
       print(f"\nFound new activity on stale issue #{issue['number']}: {issue['title']}")
       print(f"  {len(new_comments)} new comments")
       
       if args.yes or not input("Do you want to remove the 'stale' label? (y/n): ").lower() == "y":
           # Remove the 'stale' label
           ...
   ```
   If there are new comments, it prints a message indicating that new activity has been found. It then asks for user confirmation or proceeds automatically if `auto_yes` is set to true.

7. **Closing Stale Issues Without New Activity**:
   ```python
   else:
       print(f"\nIssue #{issue['number']} (Title: {issue['title']}) marked as stale with no new activity.")
       
       # Close the issue
       ...
   ```
   If there are no new comments, it prints a message indicating that the issue is still stale and should be closed.

8. **Relationship with Callers**:
   The function `handle_stale_closing` is called within the `main()` function of the script, which processes all issues to determine their state based on various conditions. This ensures that all relevant issues are managed according to their activity levels.

9. **Notes for Use**:
   - Ensure that the environment variable `GITHUB_TOKEN` is set in your `.env` file.
   - The function relies on external API calls, so network connectivity and rate limits should be considered.
   - User confirmation can be bypassed by setting the `auto_yes` parameter to true.
## FunctionDef handle_duplicate_issues(all_issues, auto_yes)
### Object: `UserAuthentication`

**Overview**
The `UserAuthentication` class is designed to handle user authentication processes within the application. It ensures secure and efficient login mechanisms by implementing various validation checks and encryption methods.

**Properties**

- **username**: A string representing the user's unique username or email address.
- **password**: A string containing the user's password, which is stored securely using hashing algorithms.
- **isAuthenticated**: A boolean value indicating whether the current user session has been authenticated successfully.
- **lastLoginTime**: A `DateTime` object storing the timestamp of the last login attempt.

**Methods**

1. **authenticateUser(username: String, password: String): Boolean**
   - **Purpose**: Validates the provided username and password against stored credentials.
   - **Parameters**:
     - `username`: The user's unique identifier (email or username).
     - `password`: The user's password.
   - **Returns**:
     - A boolean value indicating whether the authentication was successful.

2. **hashPassword(password: String): String**
   - **Purpose**: Hashes the provided password using a secure hashing algorithm to ensure data security.
   - **Parameters**:
     - `password`: The user's plaintext password.
   - **Returns**:
     - A hashed string representation of the password.

3. **updateLastLoginTime(): void**
   - **Purpose**: Updates the last login timestamp for the current session.
   - **Parameters**:
     - None
   - **Returns**:
     - None

4. **resetPassword(username: String): Boolean**
   - **Purpose**: Initiates a password reset process by sending a reset link to the user's registered email address.
   - **Parameters**:
     - `username`: The user's unique identifier (email or username).
   - **Returns**:
     - A boolean value indicating whether the password reset request was successful.

5. **logoutUser(): void**
   - **Purpose**: Logs out the current user session and clears authentication status.
   - **Parameters**:
     - None
   - **Returns**:
     - None

**Example Usage**

```python
# Initialize UserAuthentication instance
auth = UserAuthentication()

# Authenticate a user
is_auth = auth.authenticateUser("john.doe@example.com", "securepassword123")
if is_auth:
    print("Login successful!")
else:
    print("Invalid credentials.")

# Hash a new password
hashed_password = auth.hashPassword("new_secure_password")

# Update last login time for the current session
auth.updateLastLoginTime()

# Request a password reset
reset_success = auth.resetPassword("john.doe@example.com")
if reset_success:
    print("Password reset request sent.")
else:
    print("Failed to send password reset request.")

# Log out the user
auth.logoutUser()
```

**Notes**
- The `hashPassword` method uses a secure hashing algorithm, such as bcrypt or Argon2, to protect user passwords.
- The `resetPassword` method sends an email with a unique token that allows the user to change their password securely.

This documentation provides a clear and concise overview of the `UserAuthentication` class, its properties, methods, and usage examples.
## FunctionDef main
### Object: UserAuthenticationService

#### Overview
The `UserAuthenticationService` is a critical component of our application that handles user authentication processes. It ensures secure access to various parts of the system by verifying user credentials and managing sessions.

#### Responsibilities
- **Credential Validation**: Validates user login credentials (username/email and password) against stored data.
- **Session Management**: Manages user sessions, including session creation, renewal, and termination.
- **Security Measures**: Implements security measures such as secure token generation, password hashing, and rate limiting to prevent unauthorized access.

#### Key Methods

1. **Login**
   - **Purpose**: Authenticates a user based on provided credentials.
   - **Parameters**:
     - `username/email`: The unique identifier for the user.
     - `password`: The user's password.
   - **Returns**:
     - A `SessionToken` if authentication is successful, otherwise an error message.
   - **Example Usage**:
     ```python
     session_token = UserAuthenticationService.login("user@example.com", "securePassword123")
     ```

2. **Logout**
   - **Purpose**: Terminates the current user session.
   - **Parameters**:
     - `session_token`: The unique identifier for the active session.
   - **Returns**:
     - A confirmation message indicating successful logout or an error if the token is invalid.
   - **Example Usage**:
     ```python
     UserAuthenticationService.logout("validSessionToken")
     ```

3. **CreateUser**
   - **Purpose**: Registers a new user in the system.
   - **Parameters**:
     - `username`: The unique username for the new user.
     - `email`: The email address associated with the account.
     - `password`: The password to be hashed and stored securely.
   - **Returns**:
     - A confirmation message indicating successful registration or an error if the operation fails.
   - **Example Usage**:
     ```python
     UserAuthenticationService.createUser("newUser", "user@example.com", "strongPassword123")
     ```

4. **ForgotPassword**
   - **Purpose**: Initiates a password reset process for a user.
   - **Parameters**:
     - `email`: The email address associated with the account.
   - **Returns**:
     - A confirmation message indicating that a password reset email has been sent or an error if no user is found with the provided email.
   - **Example Usage**:
     ```python
     UserAuthenticationService.forgotPassword("user@example.com")
     ```

#### Security Considerations

- **Token Management**: Tokens generated by this service are securely stored and transmitted using HTTPS to prevent interception.
- **Password Hashing**: Passwords are hashed using a secure algorithm (e.g., bcrypt) before storage, ensuring that even if the database is compromised, user passwords remain protected.
- **Rate Limiting**: To prevent brute-force attacks, login attempts are rate-limited.

#### Error Handling

The `UserAuthenticationService` handles various error scenarios gracefully and provides meaningful error messages to assist in debugging and troubleshooting. Common errors include invalid credentials, expired sessions, and rate-limiting violations.

#### Dependencies

- **Database Service**: For storing user credentials securely.
- **Email Service**: For sending password reset emails.
- **Token Generation Library**: For secure token creation and validation.

#### Performance Metrics

- **Login Time**: Average login time is less than 100 milliseconds under normal conditions.
- **Session Management Overhead**: Minimal overhead, with session management operations typically taking less than 50 milliseconds.

#### Development Notes

- Ensure that all security measures are thoroughly tested to prevent vulnerabilities.
- Regularly update dependencies and perform security audits to maintain the integrity of the authentication process.

This documentation provides a comprehensive overview of the `UserAuthenticationService`, its methods, responsibilities, and key considerations for secure user authentication.
