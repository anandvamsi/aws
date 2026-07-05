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


agent core deploy
```
