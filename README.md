# Terminal GPT 

### Very Early Development (EXPECT BUGS)

This task management system leverages OpenAI's GPT-3 or GPT-4 and Pinecone's vector similarity search to automate task management, including task generation, agent assignment, and task prioritization. Forked from babyagi, the main goal of this repo is to give GPT full access to the terminal. By giving GPT access to the terminal there would no longer be a limit on it's abilities: react development, internet search, optimizing computer settings, improved file management/organization. LITERALLY NO LIMITS TO WHAT CAN BE DONE

# Proof of Concept

### Terminal Access

<img width="555" alt="Screen Shot 2023-04-08 at 12 41 24 AM" src="https://user-images.githubusercontent.com/97474920/230705402-a7818e13-bd3e-403f-b6ab-1290d14358a2.png">

Here's a proof of concept, GPT writing directly into the terminal to create a basic React App

<img width="809" alt="Screen Shot 2023-04-08 at 12 09 36 AM" src="https://user-images.githubusercontent.com/97474920/230704546-c55f437b-49c5-44fb-b24c-e45691e3b99a.png">

### Python Scripting 

OBJECTIVE='Write a python script that solves a simple task'
FIRST_TASK='Write a python script'

This is a very basic example but it should give you an idea on the capabilties of this system

<img width="466" alt="Screen Shot 2023-04-08 at 12 36 39 AM" src="https://user-images.githubusercontent.com/97474920/230705323-b25bcbfd-1486-4421-ab19-bc699c5c5f27.png">

<img width="351" alt="Screen Shot 2023-04-08 at 12 38 43 AM" src="https://user-images.githubusercontent.com/97474920/230705336-29f44d0a-a038-4e04-bc69-152e377720b9.png">

<img width="487" alt="Screen Shot 2023-04-08 at 12 39 14 AM" src="https://user-images.githubusercontent.com/97474920/230705348-7b7a3e36-a117-4053-950e-5d72b740eac6.png">



# Hive Mind

In the context of the Terminal GPT task management system, the shared context (also referred to as the "hive mind") is a Pinecone index that stores information about completed tasks and their results. This allows agents to access previously completed tasks and their results when working on new tasks. The shared context can be thought of as a collective knowledge base that agents can use to improve their performance.

For example, suppose an agent is assigned a new task that is similar to a previously completed task. The agent can access the shared context to retrieve information about the previous task, including its solution and any relevant context. This can help the agent generate a more accurate and efficient solution for the new task.

The shared context is updated in real-time as tasks are completed and new information is added. This allows agents to access the most up-to-date information when working on tasks. Additionally, the shared context can be used to identify patterns or trends in completed tasks, which can be used to improve the overall performance of the task management system.

# Setup

To set up the system, follow these steps:

Install the required Python libraries by running: 'pip install -r requirements.txt'.
Create a .env file with the following environment variables:
'OPENAI_API_KEY': Your OpenAI API key.
'PINECONE_API_KEY': Your Pinecone API key.
'PINECONE_ENVIRONMENT': The environment you are using for Pinecone.
'TABLE_NAME': The name of the Pinecone index for storing task results.
'SHARED_CONTEXT': The name of the Pinecone index for storing shared context.
'OBJECTIVE': The objective of your task management system.
'FIRST_TASK': The name of the first task to be completed by the system.
Run the Python script by executing: python3 babyagi.py.

# How it Works
The task management system operates in the following manner:

1. A task is added to the task list.
2. The system retrieves the first task from the task list and sends it to the main_agent function for processing.
3. The main_agent function assigns the task to an appropriate agent (based on keywords in the task name) and sends it to that agent for processing.
4. The agent completes the task and returns a result.
5. The result is enriched (if necessary) and stored in Pinecone for later retrieval.
6. The task_creation_agent function generates new tasks based on the completed task and adds them to the task list.
7. The prioritization_agent function prioritizes the task list based on the objective of the task management system.

# Task Module

## Task Class

The Task class represents a single task to be completed.

Attributes

'task_id (int)': The unique identifier of the task.
'task_name (str)': A short description of the task.
'completed (bool)': Whether the task has been completed or not.
'result (any)': The result of the task, if completed.

Methods

complete(result: any): Marks the task as completed and sets its result.

## TaskManager Class
The TaskManager class manages a list of Task objects.

Attributes

task_list (List[Task]): A list of Task objects.
Methods
add_task(task: Task): Adds a new task to the task list.
get_next_task() -> Union[Task, None]: Returns the next uncompleted task in the list or None if there are no more tasks.
has_tasks() -> bool: Returns True if there are any uncompleted tasks in the list, False otherwise.
get_tasks() -> List[Task]: Returns a list of all tasks, completed or not.
HELPER.py
The HELPER.py file contains various helper functions used throughout the project. These functions include:

openai_call: A function for making API calls to OpenAI's GPT-3 and GPT-4 models to generate natural language responses.
save_script_to_file: A function for saving a string of code to a file in a specified folder.
execute_terminal_command: A function for executing a command in the terminal.
openai_call
`openai_call(prompt: str, use_gpt4: bool = False, temperature : float = 0.5, max_tokens: int = 100) -> str`

This function makes an API call to either OpenAI's GPT-3 or GPT-4 models, depending on the use_gpt4 parameter, to generate a natural language response to the provided prompt. The temperature and max_tokens parameters can be used to adjust the response's creativity and length, respectively. The function returns the generated response as a string.

'save_script_to_file'
'save_script_to_file(code: str, filename: str, folder: str = "generated_scripts") -> None'

This function takes a string of code and saves it to a file with the specified filename in the folder directory. If the folder directory does not exist, it will be created.

'execute_terminal_command'
'execute_terminal_command(command: str) -> None'

This function takes a command string and executes it in the terminal. Any newlines in the command will be removed before execution.

# Validation Module

The validation module contains functions that validate code snippets or commands in various languages or formats:

is_valid_python_script(code: str) -> bool: Determines if the input string code is a valid Python script.
is_valid_javascript_script(code: str) -> bool: Determines if the input string code is a valid JavaScript script.
is_valid_css_script(code: str) -> bool: Determines if the input string code is a valid CSS script.
is_valid_terminal_command(code: str) -> bool: Determines if the input string code is a valid terminal command.
Agent Module
The agent module contains functions and classes that help in generating and managing AI-generated solutions to programming and research tasks.

# Agent Functions

The following functions represent different types of AI agents that can be used to generate solutions to programming and research tasks. Each function takes a task and shared context as input, generates a response using OpenAI's GPT-3 or GPT-4, and returns a string representation of the solution. If the generated solution is a valid script or command, it is saved to a file or executed, respectively.

'create_custom_agent(agent_name: str, role: str, prompt: str)': Creates a custom AI agent with the given agent_name, role, and prompt.
'create_python_developer_agent()': Creates an AI agent that generates a Python script to solve a programming task.
'create_javascript_developer_agent()': Creates an AI agent that generates a JavaScript script to solve a programming task.
'create_css_developer_agent()': Creates an AI agent that generates a CSS script to solve a programming task.
'create_researcher_agent()': Creates an AI agent that generates a research paper to help solve a research task.
'create_terminal_agent()': Creates an AI agent that generates a terminal command to solve a programming task.
'prioritization_agent(this_task_id: int, task_list: deque, OBJECTIVE: str, gpt_version: str = 'gpt-3')': Creates an AI agent that reprioritizes a list of tasks based on a given objective. It returns a string representation of the new task order.

# Improvements to be made 

More comprehensive validation: Currently, the validation module only checks if code snippets are valid in certain languages or formats. It would be helpful to have more robust validation to catch errors that may arise from incorrect syntax, type errors, and other common programming mistakes.

Improved prioritization: The prioritization_agent function could be improved by taking into account factors such as deadline, complexity, and task dependencies. This would allow for a more intelligent prioritization of tasks.

Better task management: The current implementation of the TaskManager class only allows for a single list of tasks. It would be helpful to have additional features such as the ability to group tasks into different categories or projects.

Enhanced AI agents: While the current AI agents are useful, they could be improved by incorporating more sophisticated techniques such as deep learning or reinforcement learning.

More customization options: While the system can be customized by changing the values in the .env file, it would be helpful to have a web interface or other GUI to make it easier to customize and manage the system.

Improved error handling: The system could benefit from better error handling to help diagnose and fix any issues that may arise during runtime.

More documentation: While the current documentation is helpful, there could be more detailed explanations of how the system works and how to use its various components. This would make it easier for users to understand and customize the system for their own needs.


# Conclusion

This task management system exemplifies how AI can be used to automate task management processes. By leveraging OpenAI's GPT-3 or GPT-4 and Pinecone's vector similarity search, the system can automatically generate new tasks, assign them to appropriate agents, and prioritize the task list based on the system's objective.
