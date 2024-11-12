## ClassDef EditorWholeFilePrompts
**EditorWholeFilePrompts**: The function of EditorWholeFilePrompts is to manage prompts for making comprehensive changes to source code files based on user requests.

**attributes**: 
· main_system: A template string defining the interaction with users, specifying roles and responsibilities.
· example_messages: An array containing examples of user interactions and corresponding assistant responses.
· system_reminder: A reminder for assistants on how to format file listings when suggesting changes.
· redacted_edit_message: The message returned when no code edits are needed.

**Code Description**: 
The `EditorWholeFilePrompts` class inherits from the `WholeFilePrompts` class, extending its functionality to provide a more specific set of prompts and guidelines for editing entire files. It is designed to handle requests that require significant changes or rewrites to source code files. The main purpose is to ensure that any necessary modifications are clearly communicated and accurately reflected in the output.

The `main_system` attribute provides a structured template for interactions, emphasizing roles such as acting as an expert software developer and making changes to the supplied code based on user requests. This template also includes placeholders `{lazy_prompt}` which can be customized with additional instructions or questions depending on the context of the request.

The `example_messages` array offers practical examples illustrating how a typical interaction might unfold, including both user input and assistant responses. These examples help in understanding the expected format and style for exchanges between users and assistants.

The `system_reminder` attribute serves as a guide for formatting file listings when suggesting changes to files. It ensures that any modifications are presented accurately by maintaining a consistent structure, which is crucial for clarity and precision in code editing processes.

Finally, the `redacted_edit_message` attribute provides a default message to be returned if no edits are required, preventing unnecessary outputs or confusion during interactions.

**Note**: 
- Users should ensure that their requests are clear and specific when interacting with the system.
- Assistants must adhere strictly to the formatting guidelines provided in `system_reminder` for file listings.
- The `example_messages` can be used as references to understand typical interaction flows, but actual prompts may vary based on user input.
