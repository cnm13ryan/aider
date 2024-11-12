## FunctionDef main
**main**: The function of main is to execute benchmarking commands for different models and edit formats.
**parameters**: 
· parameter1: `models` - A list of strings representing the model names (e.g., "gpt-3.5-turbo-0613") to be used in the benchmarking process.
· parameter2: `edit_formats` - A list of strings indicating the editing formats (e.g., "diff", "whole") for the benchmarking.

**Code Description**: The main function iterates over a predefined list of models and edit formats, constructing and executing benchmarking commands. It ensures that certain conditions are met before running each command by skipping specific combinations or setting up directory names appropriately.

The function begins by defining two lists: `models` and `edit_formats`. These lists contain the model names and editing formats to be used in the benchmarking process. The comments within the code indicate which models and edit formats are currently active, with some being commented out for potential future use.

A nested loop is then used to iterate over each combination of a model and an edit format. For each combination:
1. A directory name (`dirname`) is constructed based on the current model and edit format.
2. The `run` function is called with the appropriate parameters, including the directory name, model, and edit format.

The `run` function constructs a command list using these parameters and prints it to the console for debugging purposes. It then executes the benchmarking script located at `"./benchmark/benchmark.py"` in a non-blocking manner using `subprocess.run()`. The command includes options such as the number of threads (`--threads 10`) and a continuation flag (`--cont`).

The main function also includes conditional logic to skip certain combinations based on specific conditions. For example, it skips running benchmarking for models with the "-func" suffix when using the "-03" version.

**Note**: Ensure that the paths specified in the command are correct relative to where this script is being executed. Verify that the models and edit formats provided match those expected by the benchmarking script. The directory names should be appropriately set up to store or access model-related files for each combination of model and edit format.
## FunctionDef run(dirname, model, edit_format)
**run**: The function of `run` is to execute a benchmarking command using specified parameters.
**parameters**:
· parameter1: `dirname` - A string representing the directory name where the model and edit format related files will be stored or accessed.
· parameter2: `model` - A string specifying the model (e.g., "gpt-3.5-turbo-0613") to be used in the benchmarking process.
· parameter3: `edit_format` - A string indicating the editing format (e.g., "diff", "whole") for the benchmarking.

**Code Description**: The function `run` constructs and executes a command that runs the benchmarking script located at `"./benchmark/benchmark.py"` with specified parameters. It assembles these parameters into a command list, prints it to the console, and then uses the `subprocess.run()` method to execute the command in a non-blocking manner.

The command is built using the following elements:
- The path to the benchmarking script.
- The directory name (`dirname`).
- Additional options such as model type (`--model`), edit format (`--edit-format`), number of threads (`--threads 10`), and continuation flag (`--cont`).

The `print(" ".join(cmd))` statement outputs the command to be executed, which is useful for debugging or understanding what exactly will run. The `subprocess.run(cmd, check=True)` ensures that if the command fails, an exception will be raised.

In the context of how this function is used within the project:
- It is called by the `main` function in `benchmark/rungrid.py/main`.
- `main` iterates over a list of models and edit formats to run the benchmarking for each combination.
- The `run` function ensures that specific conditions are met, such as skipping certain combinations or setting up directory names.

**Note**: Ensure that the paths specified in the command (e.g., `"./benchmark/benchmark.py"`) are correct relative to where this script is being executed. Also, verify that the models and edit formats provided match those expected by the benchmarking script.
