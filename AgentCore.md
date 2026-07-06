AgentCore

# What is AgentCore


# what problem does the AgentCore solve


# How to install Agentcore


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

