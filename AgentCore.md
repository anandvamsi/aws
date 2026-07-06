AgentCore

# What is AgentCore
Amazon Bedrock AgentCore is an all-in-one platform from AWS designed for deploying, operating, and scaling AI agents in a secure, enterprise-grade cloud environment.

It is completely framework-agnostic (works with LangGraph, CrewAI, LlamaIndex, etc.) and model-agnostic (works with Claude, Llama, OpenAI, etc.). Instead of rewriting code for a specific cloud environment, you build your agent using the framework you prefer, and AgentCore provides the underlying infrastructure to run it.


# what problem does the AgentCore solve
When developers move an AI agent from a local laptop prototype to a production environment, they typically run into heavy structural and operational challenges. AgentCore is built specifically to solve these five main pain points:

## Eliminates Infrastructure & Scaling Complexity
### The Problem
Deploying an agent traditionally requires setting up server clusters (like Kubernetes), configuring Docker containers, building API Gateways, and managing load balancers to handle sudden traffic spikes.  

### Solution
AgentCore provides a fully managed, serverless Runtime. It packages your agent code, manages the containerization, and scales microVM resources up or down automatically based on demand. You pay only for execution time. 

### Resolves Serverless Timeouts during Long Reasoning Loops

## The Problem: 
Standard serverless functions (like AWS Lambda) have maximum timeout limits (typically 15 minutes). AI agents executing complex, multi-step tasks or long reasoning loops frequently hit these limits and crash.

## The Solution: 
The AgentCore Runtime maintains persistent user sessions. MicroVMs stay active for the entire duration of an agent-user conversation, handling extended, multi-step execution paths without timing out.


## Standardizes Complex Tool & API Integration
### The Problem: 
Connecting an AI agent to multiple databases, legacy enterprise APIs, or internal microservices requires writing massive amounts of custom wrapper code and API translators.

### The Solution: 
The AgentCore Gateway serves as a centralized hub. It automatically translates standard AWS Lambda functions, external APIs, and Model Context Protocol (MCP) servers into clean tool schemas that any AI model can immediately understand and interact with.


## Handles Enterprise Security, Identity, and Guardrails 

### The Problem: Keeping track of user authentication (e.g., verifying who is talking to the agent) and ensuring the agent doesn't access unauthorized corporate data requires building complex IAM or OAuth systems from scratch.  

### The Solution: AgentCore Identity natively manages inbound and outbound authentication (integrating with Okta, Entra, Cognito, etc.) so agents can act safely on a specific user's behalf.  Policy Control allows you to write natural language rules (e.g., "Block all refunds over $1,000") that AgentCore enforces instantly at the infrastructure level, preventing the LLM from making unauthorized system calls.


# How to install Agentcore
```bash
npm install -g @aws/agentcore
or
pip uninstall bedrock-agentcore-starter-toolkit
```


## step 1
agentcore create
```
  AgentCore Create
  Project: mcpdemo
  [done]    Create mcpdemo/ project directory
  [done]    Prepare agentcore/ directory
  [done]    Initialize git repository
  Created:
    mcpdemo/
      agentcore/  Config and CDK project
  Next: Run agentcore add to add an agent
  Esc back · Ctrl+C quit
Update available: 0.21.1 → 0.22.0
```

## Step 2
we can add any AgentCore component, In this case im adding gateway
```bash
agentcore add gateway   --name MyGateway   --authorizer-type NONE
Added gateway 'MyGateway'
```

## Step 3 Deployment 
what does the Agentcore deploy do
```
Local Project
    |
    | agentcore add gateway
    v
Project Configuration (local)
    |
    | agentcore deploy
    v
AWS AgentCore Gateway (actual cloud resource
```

#### Note prior to deployment the IAM role or IAM user which you using to authneticate needs 



### Agent core deploy
```
agentcore deploy
AgentCore Deploy

  Project: mcpdemo
  Target: us-west-2:507250483285

  [done]    Validate project
  [done]    Check dependencies
  [done]    Build CDK project
  [done]    Synthesize CloudFormation
  [done]    Check stack status
  [done]    Bootstrap AWS environment
  [done]    Skip diff (new stack)
  [done]    Publish assets
  [done]    Persist deployment state

  ╭────────────────────────────────────────────────╮
  │ ✓ Deploy to AWS Complete                       │
  │                                                │
  │ [████████████████████] 4/4                     │
  ╰────────────────────────────────────────────────╯

  Log: agentcore/.cli/logs/deploy/deploy-20260706-110829.log

  Next: Run agentcore add to add an agent, or agentcore status to view deployment status

  Esc back · Ctrl+C quit

Update available: 0.21.1 → 0.22.0
Run `npm install -g @aws/agentcore@latest` to update.
```

## Inoder to talk to the application we need to create a tool.json in the same project folder, Here is the tools.json for this project
```bash
[
  {
    "name": "get_customer_discount",
    "description": "Retrieves the internal corporate retail discount tier and percentage for a user using their unique customer ID.",
    "inputSchema": {
      "type": "object",
      "properties": {
        "customerId": {
          "type": "string",
          "description": "The unique system customer identifier used for lookup. For example: 'VIP-404' or 'STD-881'."
        }
      },
      "required": [
        "customerId"
      ]
    }
  }
]
```


##  To check the status of the agent
```bash

#agentcore status
AgentCore Status (target: default, us-west-2)

Gateways
  MyGateway: Deployed (mcpdemo-mygateway-j3lbzveuur)

Update available: 0.21.1 → 0.22.0
Run `npm install -g @aws/agentcore@latest` to update.
```

## Attaching the target Lambda
```
agentcore add gateway-target   --name MCP-gw-test-AgentCore-Target   --type lambda-function-arn   --lambda-arn arn:aws:lambda:us-west-2:50725XXXX85:function:MCP-AgentCore_test   --tool-schema-file tools.json   --gateway MyGateway
Added gateway target 'MCP-gw-test-AgentCore-Target'
```

Agent core deploy
```
agentcore deploy
AgentCore Deploy

  Project: mcpdemo
  Target: us-west-2:507250483285

  [done]    Validate project
  [done]    Check dependencies
  [done]    Build CDK project
  [done]    Synthesize CloudFormation
  [done]    Check stack status
  [done]    Bootstrap AWS environment
  [done]    Skip diff (new stack)
  [done]    Publish assets
  [done]    Persist deployment state

  ╭────────────────────────────────────────────────╮
  │ ✓ Deploy to AWS Complete                       │
  │                                                │
  │ [████████████████████] 4/4                     │
  ╰────────────────────────────────────────────────╯

  Log: agentcore/.cli/logs/deploy/deploy-20260706-110829.log

  Next: Run agentcore add to add an agent, or agentcore status to view deployment status

  Esc back · Ctrl+C quit

Update available: 0.21.1 → 0.22.0
Run `npm install -g @aws/agentcore@latest` to update.
```


## Grab the  gateway URL and create the mcp.json and configure to client
```bash
{
  "mcpServers": {
    "agentcore-lambda-tools": {
      "command": "curl",
      "args": [
        "-X", "POST",
        "https://mcpdemo-mygateway-j3lbzveuur.gateway.bedrock-agentcore.us-west-2.amazonaws.com",
        "-H", "Content-Type: application/json",
        "-d", "@-"
      ]
    }
  }
}
```

