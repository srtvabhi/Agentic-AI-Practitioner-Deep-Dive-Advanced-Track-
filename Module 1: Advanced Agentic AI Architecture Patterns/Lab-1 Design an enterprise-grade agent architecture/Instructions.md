# Module 1 Lab Guide
# OpenAI Agent SDK with Azure AI Foundry (GPT-4.1)

> **Module:** OpenAI Agent SDK Fundamentals  
> **Lab:** Design an Enterprise-Grade Agent Architecture  
> **Model:** Azure AI Foundry GPT-4.1  
> **SDK:** OpenAI Agent SDK

---

# Lab Overview

In this lab, you will build an **Enterprise IT Service Desk** using the **OpenAI Agent SDK** and **Azure AI Foundry GPT-4.1**.

The application demonstrates how multiple AI agents collaborate to solve a real-world enterprise problem through **agent orchestration** and **agent handoffs**.

---

# Learning Objectives

By the end of this lab, you will be able to:

- Configure the OpenAI Agent SDK with Azure AI Foundry
- Create and activate a Python virtual environment
- Manage project dependencies using `requirements.txt`
- Load Azure credentials securely using a `.env` file
- Build specialized AI agents
- Implement agent handoffs
- Design a scalable enterprise multi-agent architecture

---

# Prerequisites

## Software

- Python 3.10 or later
- Visual Studio Code (Recommended)

## Azure Resources

- Azure AI Foundry Project
- GPT-4.1 Model Deployment
- Azure AI Foundry Endpoint
- Azure AI Foundry API Key

---

# Project Setup

## Step 1 — Create the Project Folder

```bash
mkdir Enterprise_Agent_Lab
cd Enterprise_Agent_Lab
```

---

## Step 2 — Create a Virtual Environment

### Windows

```bash
python -m venv venv

venv\Scripts\activate
```

### Linux / macOS

```bash
python3 -m venv venv

source venv/bin/activate
```

---

## Step 3 — Create `requirements.txt`

Create a file named:

```
requirements.txt
```

Add the following packages:

```text
openai-agents
python-dotenv
```

---

## Step 4 — Install Dependencies

```bash
pip install -r requirements.txt
```

Verify installation:

```bash
pip list
```

---

# Project Folder Structure

Keep **all project files inside a single folder**.

```text
Enterprise_Agent_Lab/
│
├── venv/
├── .env
├── requirements.txt
└── enterprise_agent_architecture.py
```

---

# Configure Azure AI Foundry

## Step 5 — Create the `.env` File

Create a file named:

```
.env
```

Add your Azure AI Foundry credentials.

```env
AZURE_OPENAI_API_KEY=YOUR_API_KEY

AZURE_OPENAI_ENDPOINT=https://YOUR-RESOURCE.services.ai.azure.com

AZURE_OPENAI_DEPLOYMENT=gpt-4.1
```

> **Note**
>
> Replace the placeholder values with your Azure AI Foundry credentials.

---

# Lab 1
# Design an Enterprise-Grade Agent Architecture

---

# Objective

Build a **multi-agent Enterprise IT Service Desk**.

The application consists of one coordinator agent and three specialized agents.

---

# Enterprise Architecture

```text
                    User
                      │
                      ▼
             Coordinator Agent
                      │
        ┌─────────────┼─────────────┐
        │             │             │
        ▼             ▼             ▼
Classification   Knowledge      Routing
    Agent          Agent          Agent
        │             │             │
        └─────────────┼─────────────┘
                      │
                      ▼
               Final Response
```

---

# Agent Responsibilities

## Coordinator Agent

- Receives the user request
- Delegates work to specialized agents
- Combines outputs
- Returns the final response

---

## Classification Agent

Determines the issue category.

Example categories:

- Network
- Hardware
- Application
- Security

---

## Knowledge Agent

Provides troubleshooting guidance.

Example:

- Verify VPN access
- Check Internet connectivity
- Validate login credentials

---

## Routing Agent

Determines the appropriate support team.

Example teams:

- Network Operations
- Security Team
- Infrastructure Team
- Application Support

---

# Learning Outcomes

After completing this lab you will understand:

- Multi-agent architecture
- Agent decomposition
- Agent handoffs
- Enterprise workflow orchestration
- Azure AI Foundry integration
- OpenAI Agent SDK fundamentals

---

# Source Code

Create the file:

```
enterprise_agent_architecture.py
```

```python
import os

from dotenv import load_dotenv

from agents import Agent
from agents import Runner
from agents import OpenAIChatCompletionsModel

# ------------------------------------
# Load Environment Variables
# ------------------------------------

load_dotenv()

AZURE_OPENAI_API_KEY = os.getenv("AZURE_OPENAI_API_KEY")
AZURE_OPENAI_ENDPOINT = os.getenv("AZURE_OPENAI_ENDPOINT")
AZURE_OPENAI_DEPLOYMENT = os.getenv("AZURE_OPENAI_DEPLOYMENT")

# ------------------------------------
# Configure Azure AI Foundry Model
# ------------------------------------

model = OpenAIChatCompletionsModel(
    model=AZURE_OPENAI_DEPLOYMENT,
    api_key=AZURE_OPENAI_API_KEY,
    base_url=AZURE_OPENAI_ENDPOINT
)

# ------------------------------------
# Classification Agent
# ------------------------------------

classification_agent = Agent(
    name="Classification Agent",
    instructions="""
Classify incoming tickets into one of the following categories:

- Network
- Hardware
- Application
- Security
"""
)

# ------------------------------------
# Knowledge Agent
# ------------------------------------

knowledge_agent = Agent(
    name="Knowledge Agent",
    instructions="""
Provide troubleshooting steps for the identified issue.
"""
)

# ------------------------------------
# Routing Agent
# ------------------------------------

routing_agent = Agent(
    name="Routing Agent",
    instructions="""
Determine which enterprise support team should receive the ticket.
"""
)

# ------------------------------------
# Coordinator Agent
# ------------------------------------

coordinator = Agent(
    name="Coordinator Agent",
    instructions="""
Coordinate the IT support workflow.

Workflow:

1. Classify the issue.
2. Retrieve troubleshooting guidance.
3. Determine the support team.
4. Return the final response.
""",
    handoffs=[
        classification_agent,
        knowledge_agent,
        routing_agent
    ],
    model=model
)

# ------------------------------------
# Execute Workflow
# ------------------------------------

result = Runner.run_sync(
    coordinator,
    "My laptop cannot connect to VPN."
)

print(result.final_output)
```

---

# Execute the Lab

Run the application.

```bash
python enterprise_agent_architecture.py
```

---

# Expected Output

```text
Category: Network

Troubleshooting Steps

1. Verify VPN access
2. Check Internet connection
3. Validate user credentials

Assigned Team

Network Operations Team
```

---

# Final Project Structure

```text
Enterprise_Agent_Lab/
│
├── venv/
├── .env
├── requirements.txt
└── enterprise_agent_architecture.py
```

---

# Lab Summary

Congratulations!

You have successfully built an **Enterprise Multi-Agent Architecture** using:

- Azure AI Foundry GPT-4.1
- OpenAI Agent SDK
- Python Virtual Environment
- Environment Variables (`.env`)
- Dependency Management (`requirements.txt`)
- Agent Handoffs
- Enterprise Workflow Orchestration

This project serves as a foundation for building scalable enterprise AI applications using the OpenAI Agent SDK.
