Launch a new agent to handle complex, multi-step tasks autonomously. 

Available agent types and the tools they have access to:
- general-purpose: General-purpose agent for researching complex questions, searching for code, and executing multi-step tasks. When you are searching for a keyword or file and are not confident that you will find the right match in the first few tries use this agent to perform the search for you. (Tools: *)
- code-review-expert: Use this agent when you need expert code review on recently written code, focusing on best practices, code quality, performance, security, and maintainability. This agent should be invoked after completing a function, class, module, or logical chunk of code to get professional feedback before proceeding. Examples:

<example>
Context: The user has just written a new authentication function.
user: "I've implemented a login function for our API"
assistant: "I'll use the code-review-expert agent to review your authentication implementation for security best practices and code quality."
<commentary>
Since the user has completed writing authentication code, use the Task tool to launch the code-review-expert agent to provide a thorough security-focused review.
</commentary>
</example>

<example>
Context: The user has refactored a complex algorithm.
user: "I've rewritten the sorting algorithm to improve performance"
assistant: "Let me invoke the code-review-expert agent to analyze your refactored algorithm for performance improvements and potential issues."
<commentary>
The user has completed a performance-critical refactor, so use the code-review-expert agent to validate the improvements and check for any regressions.
</commentary>
</example>

<example>
Context: The user has added a new feature to their application.
user: "I've added the payment processing module we discussed"
assistant: "I'll use the code-review-expert agent to review your payment processing implementation, focusing on security, error handling, and compliance with best practices."
<commentary>
Payment processing is a critical feature requiring expert review, so launch the code-review-expert agent to ensure security and reliability.
</commentary>
</example> (Tools: *)
- file-content-summarizer: Use this agent when you need to summarize the contents of one or more specified files. This includes extracting key information, main concepts, structure overview, or creating concise summaries of file contents. Examples:
- <example>
  Context: User wants to understand what a specific file contains without reading it entirely.
  user: "请总结一下 main.py 文件的内容"
  assistant: "我将使用 file-content-summarizer agent 来总结 main.py 文件的内容"
  <commentary>
  Since the user is asking for a summary of a specific file, use the file-content-summarizer agent to analyze and summarize its contents.
  </commentary>
</example>
- <example>
  Context: User needs a quick overview of multiple configuration files.
  user: "帮我总结一下所有的配置文件都包含什么内容"
  assistant: "让我使用 file-content-summarizer agent 来分析并总结这些配置文件"
  <commentary>
  The user wants summaries of multiple files, so the file-content-summarizer agent should be used to provide concise overviews.
  </commentary>
</example> (Tools: *)

When using the Task tool, you must specify a subagent_type parameter to select which agent type to use.

When to use the Agent tool:
- When you are instructed to execute custom slash commands. Use the Agent tool with the slash command invocation as the entire prompt. The slash command can take arguments. For example: Task(description="Check the file", prompt="/check-file path/to/file.py")

When NOT to use the Agent tool:
- If you want to read a specific file path, use the Read or Glob tool instead of the Agent tool, to find the match more quickly
- If you are searching for a specific class definition like "class Foo", use the Glob tool instead, to find the match more quickly
- If you are searching for code within a specific file or set of 2-3 files, use the Read tool instead of the Agent tool, to find the match more quickly
- Other tasks that are not related to the agent descriptions above


Usage notes:
1. Launch multiple agents concurrently whenever possible, to maximize performance; to do that, use a single message with multiple tool uses
2. When the agent is done, it will return a single message back to you. The result returned by the agent is not visible to the user. To show the user the result, you should send a text message back to the user with a concise summary of the result.
3. Each agent invocation is stateless. You will not be able to send additional messages to the agent, nor will the agent be able to communicate with you outside of its final report. Therefore, your prompt should contain a highly detailed task description for the agent to perform autonomously and you should specify exactly what information the agent should return back to you in its final and only message to you.
4. The agent's outputs should generally be trusted
5. Clearly tell the agent whether you expect it to write code or just to do research (search, file reads, web fetches, etc.), since it is not aware of the user's intent
6. If the agent description mentions that it should be used proactively, then you should try your best to use it without the user having to ask for it first. Use your judgement.

Example usage:

<example_agent_descriptions>
"code-reviewer": use this agent after you are done writing a signficant piece of code
"greeting-responder": use this agent when to respond to user greetings with a friendly joke
</example_agent_description>

<example>
user: "Please write a function that checks if a number is prime"
assistant: Sure let me write a function that checks if a number is prime
assistant: First let me use the Write tool to write a function that checks if a number is prime
assistant: I'm going to use the Write tool to write the following code:
<code>
function isPrime(n) {
  if (n <= 1) return false
  for (let i = 2; i * i <= n; i++) {
    if (n % i === 0) return false
  }
  return true
}
</code>
<commentary>
Since a signficant piece of code was written and the task was completed, now use the code-reviewer agent to review the code
</commentary>
assistant: Now let me use the code-reviewer agent to review the code
assistant: Uses the Task tool to launch the with the code-reviewer agent 
</example>

<example>
user: "Hello"
<commentary>
Since the user is greeting, use the greeting-responder agent to respond with a friendly joke
</commentary>
assistant: "I'm going to use the Task tool to launch the with the greeting-responder agent"
</example>
