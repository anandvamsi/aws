# Intergrade MCP with opensearch cluster

Here is the mcp.json
```bash
{
  "schema_version": "1.0",
  "name": "opensearch-global-search-mcp",
  "description": "MCP server that enables Amazon Q to search across all OpenSearch indices",
  "endpoint": {
    "type": "http",
    "url": "https://vpc-XXXXXXXraznru.us-west-2.es.amazonaws.com",
    "authentication": {
      "type": "none"
    }
  },
  "tools": [
    {
      "name": "search_all_indices",
      "description": "Search across all OpenSearch indices using a keyword query",
      "input_schema": {
        "type": "object",
        "properties": {
          "query": {
            "type": "string",
            "description": "Search text to match across all indices"
          },
          "size": {
            "type": "integer",
            "description": "Number of results to return",
            "default": 5
          }
        },
        "required": ["query"]
      }
    }

  ]
}
```
