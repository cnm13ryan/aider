## ClassDef MarkdownStream
**MarkdownStream**: The function of MarkdownStream is to display live updates of Markdown-formatted text in a terminal interface.

**attributes**:
- `live`: Represents the Live instance used to update the screen.
- `when`: Tracks the last time an update was performed, ensuring minimal delays between updates.
- `min_delay`: Sets the minimum delay before updating the screen.
- `live_window`: Determines how many lines of output are shown in live mode.
- `printed`: Stores previously printed text for comparison.

**Code Description**: 
The `MarkdownStream` class is designed to display and update Markdown-formatted text in a terminal. It initializes with an optional set of Markdown arguments, which can be customized through the `mdargs` parameter. The constructor creates a `Live` instance that refreshes at a rate controlled by `min_delay`. 

When new text is passed to the `update` method, it checks if sufficient time has elapsed since the last update (`when`). If not, it returns immediately to avoid unnecessary updates. Otherwise, it processes the text using the `Markdown` class from rich and prints it to the console with formatting applied. The output is then split into lines, and only a portion of these lines are displayed based on `live_window`. 

The `update` method manages both live and final updates:
- For live updates, it shows recent changes while hiding older ones.
- For final updates, it stops the live display and clears the screen.

**Note**: 
- Ensure that the `Live` instance is properly managed to avoid memory leaks or unexpected behavior.
- The `min_delay` should be set appropriately to balance between responsiveness and performance. 

**Output Example**: 
Consider a scenario where you are displaying an ongoing log of events in Markdown format:
```markdown
## Log

- Event 1: Started at 2023-10-01 09:00 AM
- Event 2: Completed at 2023-10-01 10:00 AM
...
```
The `MarkdownStream` would continuously update the log, showing new events as they occur and hiding older ones to maintain a clean display. When the logging is complete, it stops updating and clears the screen.
### FunctionDef __init__(self, mdargs)
**__init__**: The function of __init__ is to initialize the MarkdownStream instance and set up its initial state.
**parameters**:
· parameter1: mdargs (Optional) - A dictionary or None that contains markdown arguments to be used by the MarkdownStream instance.

**Code Description**: 
The `__init__` method in the `MarkdownStream` class initializes a new instance of the `MarkdownStream`. Here is a detailed breakdown:

- **self.printed = []**: This line initializes an empty list named `printed`, which will likely store information about what has been printed or processed by the stream.

- **if mdargs: self.mdargs = mdargs else: self.mdargs = dict()**: 
  - If `mdargs` is provided (not None), it assigns `mdargs` to `self.mdargs`. This means that any custom markdown arguments passed during object creation will be stored and used by the instance.
  - Otherwise, if no `mdargs` are provided, an empty dictionary (`dict()`) is assigned to `self.mdargs`. This ensures that even without explicit arguments, there is still a place for default or later-defined arguments.

- **self.live = Live(Text(""), refresh_per_second=1.0 / self.min_delay)**: 
  - The `Live` object from the rich library (assuming it's being used) is instantiated with an empty text and a specified refresh rate.
  - `Text("")`: Creates an empty text object to be displayed in real-time updates.
  - `refresh_per_second=1.0 / self.min_delay`: Sets the update frequency of the live stream, where `self.min_delay` should represent the minimum delay between updates.

- **self.live.start()**: Starts the live streaming process, allowing for dynamic updates and rendering of content over time.

**Note**: Ensure that the `Live` object from the rich library is properly imported at the beginning of the file. Also, make sure that `min_delay` is defined as a class attribute or method to provide meaningful delays between refreshes.
***
### FunctionDef __del__(self)
**__del__**: The function of __del__ is to clean up resources when an instance of MarkdownStream is destroyed.

**parameters**: This method does not take any parameters as it is called automatically by Python's garbage collector when an instance is about to be destroyed.

**Code Description**: 
The `__del__` method in the `MarkdownStream` class ensures that resources are properly cleaned up when an instance of this class is deleted. Specifically, if the `live` attribute (presumably a stream or similar resource) is active (`self.live` evaluates to True), the method attempts to stop it using the `stop()` method.

Here's a detailed analysis:
- **Condition Check**: The first line checks whether `self.live` is active. This condition ensures that only if there is an ongoing process (e.g., a live stream or task) should it be stopped.
- **Resource Cleanup Attempt**: If `self.live` is active, the method tries to call its `stop()` method. This is typically used for gracefully stopping any running processes or freeing up resources.
- **Exception Handling**: The try-except block around the `stop()` call ensures that if an error occurs during the stop process (e.g., due to some unexpected state), it will be caught and ignored using a pass statement. This prevents the method from raising an exception, which could otherwise cause issues in the application.

**Note**: 
- Ensure that the `live` attribute is properly defined and implements a `stop()` method for this cleanup logic to work as intended.
- Be cautious with resource management; improper handling of exceptions or stopping processes can lead to unexpected behavior. Always test thoroughly in different scenarios.
- This method is automatically invoked by Python's garbage collector when an instance goes out of scope, making it useful for managing resources that should be freed at the end of their lifecycle.
***
### FunctionDef update(self, text, final)
**update**: The function of update is to process and display updated text in a Markdown format.
**parameters**:
· parameter1: `text` - The new Markdown-formatted string that needs to be displayed.
· parameter2: `final` - A boolean indicating whether this update is the final one. If it's not the final update, the function will wait for a minimum delay before processing.

**Code Description**: 
The `update` method in the `MarkdownStream` class handles the dynamic updating of text content using Markdown formatting. Here’s a detailed breakdown:

1. **Initial Check and Delay Handling**: The function first checks if there is any pending delay based on the current time (`now`) compared to when it was last updated (`self.when`). If not final, it ensures that at least `self.min_delay` has passed since the last update before proceeding.

2. **Updating Internal State**: It updates the internal timestamp (`self.when`) with the current time and initializes a temporary string buffer using `io.StringIO()` to capture console output.

3. **Rendering Markdown Text**: The input text is rendered as a Markdown object, utilizing any additional arguments stored in `self.mdargs`.

4. **Displaying the Output**: Using the `Console` class from rich, it prints the rendered Markdown content into the string buffer (`string_io`). This captured output represents what will be displayed.

5. **Processing Lines for Display**:
   - The function splits the output text into individual lines.
   - If this is not a final update, it subtracts the `live_window` to hide some of the initial lines, focusing on new content.
   - It then calculates how many new lines need to be displayed and extracts these lines.

6. **Updating the Display**: The newly extracted lines are formatted as an ANSI text object (`Text.from_ansi()`) and printed to the live console using `self.live.console.print(show)`. This ensures that only updated content is shown.
   
7. **Finalizing the Update**:
   - If this update is final, it stops the live display by updating with an empty string and setting `self.live` to `None`.
   - Otherwise, it appends the remaining lines (omitted in non-final updates) as a continuation of the output.

**Note**: Ensure that `self.min_delay`, `self.when`, `self.mdargs`, `self.printed`, `self.live`, and other related attributes are properly initialized before calling this method. The `Console` object should be set up correctly to handle ANSI escape sequences for text formatting.

**Output Example**: 
For example, if the input text is:
```
# Hello World
This is a **Markdown** example.
- Item 1
- Item 2
```

And `self.min_delay` is 0.5 seconds, then on the first call with non-final status, it might display only "Hello World" and "This is a **Markdown** example." after a delay of 0.5 seconds. On subsequent calls or if `final=True`, all lines would be displayed immediately without further delays.
***
