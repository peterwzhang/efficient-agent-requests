# Efficient Agent Requests

A lightweight workflow that [optimizes how many actions you can get from a request](#how-this-works). This workflow was tested with Github Copilot Agent mode with Claude Sonnet 4, but can probably be used with other similar AI agents with a few small tweaks.

## Usage

To use this workflow:

1. Put the feedback.sh script into the root directory
2. Add the contents of copilot-instructions.md to your agent instructions
3. (Optional) Add additional instructions for code generation to the bottom of the instructions.
4. Prompt your AI agent and it will run the script to ask for further instructions after each task
5. Respond with further instruction (you can also ask it to use other tools here)

In order to stop, just tell the agent to stop when it asks you for the next instructions.

## How this works?

In Github Copilot everytime you send a prompt in the chat, it uses a premium request ([source](https://docs.github.com/en/copilot/managing-copilot/understanding-and-managing-copilot-usage/understanding-and-managing-requests-in-copilot#user-content-fn-2)). A tool call does not count as a request in agent mode for Github Copilot (and for some other AI agents). Copilot can run the script as a tool call and read the input from the script as the next instruction, allowing the agent to continue working and asking for instructions in a loop while still using the "initial" request. The script itself is just a simple shell script that asks for user instructions.

An example without the workflow:

```text
User Chat: Can you make me a login page for my website? (1 request)
AI Agent: Creates webpage
User Chat: Can you change the login button to purple? (1 request)
AI Agent: Yes, I will make that change
```

An example with the workflow:

```text
User Chat: Can you make me a login page for my website? (1 request)
AI Agent: Creates webpage, let me run the feedback script
User from Script: Can you change the login button to purple? (0 request)
AI Agent: Yes, I will make that change
```

**Note: There are usually limits in place for tools**, for Github Copilot it seems to be around 10 tool calls.
