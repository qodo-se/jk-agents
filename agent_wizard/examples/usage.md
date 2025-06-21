# Agent Wizard Usage Examples

This document provides comprehensive examples of using the Agent Wizard to create different types of agents.

## Basic Usage Examples

### 1. Interactive Mode (Default)

```bash
# Start the wizard and follow the prompts
qodo agent_wizard
```

**Sample Session:**
```
🧙‍♂️ Welcome to the Agent Wizard!

? What would you like to name your agent? 
> file-organizer

? What is the primary purpose of your agent?
> Organize files in directories based on type, date, or custom rules

? What types of inputs will your agent receive?
> Directory paths, file patterns, organization rules

? What outputs should it produce?
> Organized directory structure, summary report of moved files

? What tools or services does it need to integrate with?
> File system operations, possibly cloud storage

✨ Creating your agent...
```

### 2. Quick Creation with Parameters

```bash
# Create agent with basic info provided
qodo agent_wizard \
  --agent_name=csv-processor \
  --agent_purpose="Process CSV files and generate analytics reports"
```

### 3. Non-Interactive Mode

```bash
# Skip interactive questions (uses defaults and provided parameters)
qodo agent_wizard \
  --agent_name=backup-manager \
  --agent_purpose="Automated backup management and verification" \
  --interactive=false
```

## Agent Type Examples

### 1. Data Processing Agent

```bash
qodo agent_wizard --agent_name=data-transformer
```

**Wizard Questions & Answers:**
- **Purpose**: Transform data between different formats (JSON, CSV, XML, YAML)
- **Inputs**: Data files, transformation rules, output format specifications
- **Outputs**: Transformed data files, validation reports, error logs
- **Tools**: File system, data validation libraries, format converters
- **Success Criteria**: All data transformed without loss, validation passes

**Generated Configuration Highlights:**
```toml
[commands.data_transformer]
description = "Transform data between different formats with validation"

arguments = [
    { name = "input_file", type = "string", required = true, description = "Path to input data file" },
    { name = "output_format", type = "string", required = true, description = "Target format (json, csv, xml, yaml)" },
    { name = "validation_rules", type = "string", required = false, description = "Path to validation rules file" },
    { name = "output_directory", type = "string", required = false, default = "./output", description = "Output directory" }
]

available_tools = ["filesystem", "shell"]
```

### 2. API Integration Agent

```bash
qodo agent_wizard --agent_name=api-orchestrator
```

**Wizard Questions & Answers:**
- **Purpose**: Orchestrate multiple API calls and aggregate results
- **Inputs**: API endpoints, authentication tokens, request parameters
- **Outputs**: Aggregated API responses, error reports, performance metrics
- **Tools**: HTTP client, authentication handling, data aggregation
- **Integrations**: Various REST APIs, possibly GraphQL endpoints

**Generated Configuration Highlights:**
```toml
mcpServers = """
{
  "mcpServers": {
    "web": {
      "command": "uvx",
      "args": ["mcp-web-server"]
    },
    "shell": {
      "command": "uvx", 
      "args": ["mcp-shell-server"]
    }
  }
}
"""

arguments = [
    { name = "api_config", type = "string", required = true, description = "Path to API configuration file" },
    { name = "auth_token", type = "string", required = false, description = "Authentication token" },
    { name = "timeout", type = "number", required = false, default = 30, description = "Request timeout in seconds" }
]
```

### 3. Code Analysis Agent

```bash
qodo agent_wizard --agent_name=security-scanner
```

**Wizard Questions & Answers:**
- **Purpose**: Scan code for security vulnerabilities and compliance issues
- **Inputs**: Source code directories, configuration files, security rules
- **Outputs**: Security reports, vulnerability lists, compliance scores
- **Tools**: Static analysis tools, security scanners, report generators
- **CI/CD**: Yes, integrate with build pipelines

**Generated Configuration Highlights:**
```toml
mcpServers = """
{
  "mcpServers": {
    "shell": {
      "command": "uvx",
      "args": ["mcp-shell-server"],
      "env": {
        "ALLOW_COMMANDS": "find,grep,git,npm,pip,bandit,eslint,sonarqube,semgrep"
      }
    }
  }
}
"""

exit_expression = "security_score >= 8.0 && critical_issues == 0"
```

### 4. Documentation Generator

```bash
qodo agent_wizard --agent_name=doc-generator
```

**Wizard Questions & Answers:**
- **Purpose**: Generate documentation from code comments and API specifications
- **Inputs**: Source code files, API specs, documentation templates
- **Outputs**: HTML/Markdown documentation, API reference, user guides
- **Tools**: Documentation generators, template engines, file processors
- **Formats**: Support multiple output formats (HTML, PDF, Markdown)

**Generated Configuration Highlights:**
```toml
arguments = [
    { name = "source_directory", type = "string", required = true, description = "Source code directory" },
    { name = "output_format", type = "string", required = false, default = "html", description = "Output format (html, markdown, pdf)" },
    { name = "template", type = "string", required = false, description = "Documentation template to use" },
    { name = "include_private", type = "boolean", required = false, default = false, description = "Include private methods/functions" }
]
```

## Advanced Configuration Examples

### 1. Multi-Tool Integration

```bash
qodo agent_wizard --agent_name=deployment-manager
```

**Complex MCP Configuration:**
```toml
mcpServers = """
{
  "mcpServers": {
    "shell": {
      "command": "uvx",
      "args": ["mcp-shell-server"],
      "env": {
        "ALLOW_COMMANDS": "docker,kubectl,helm,terraform,ansible,git,ssh,scp"
      }
    },
    "aws": {
      "command": "uvx",
      "args": ["mcp-aws-server"],
      "env": {
        "AWS_REGION": "${AWS_REGION}",
        "AWS_ACCESS_KEY_ID": "${AWS_ACCESS_KEY_ID}",
        "AWS_SECRET_ACCESS_KEY": "${AWS_SECRET_ACCESS_KEY}"
      }
    },
    "database": {
      "command": "uvx",
      "args": ["mcp-database-server"],
      "env": {
        "DATABASE_URL": "${DATABASE_URL}"
      }
    }
  }
}
"""
```

### 2. Custom Output Schema

```bash
qodo agent_wizard --agent_name=performance-analyzer
```

**Complex Output Schema:**
```json
{
  "type": "object",
  "properties": {
    "performance_metrics": {
      "type": "object",
      "properties": {
        "response_time": { "type": "number", "description": "Average response time in ms" },
        "throughput": { "type": "number", "description": "Requests per second" },
        "error_rate": { "type": "number", "description": "Error rate percentage" },
        "cpu_usage": { "type": "number", "description": "CPU usage percentage" },
        "memory_usage": { "type": "number", "description": "Memory usage in MB" }
      }
    },
    "bottlenecks": {
      "type": "array",
      "items": {
        "type": "object",
        "properties": {
          "component": { "type": "string" },
          "severity": { "type": "string", "enum": ["low", "medium", "high", "critical"] },
          "description": { "type": "string" },
          "recommendation": { "type": "string" }
        }
      }
    },
    "recommendations": {
      "type": "array",
      "items": { "type": "string" }
    }
  }
}
```

## Validation Examples

### 1. Test Scenario Creation

The wizard automatically creates test scenarios based on your agent's purpose:

```yaml
# Generated test scenarios for a log-analyzer agent
test_scenarios:
  - name: "Parse standard log file"
    description: "Test parsing of a standard Apache log file"
    input:
      log_file: "examples/apache.log"
      format: "apache_common"
    expected_output:
      entries_parsed: 1000
      errors_found: 5
      warnings_found: 23
      status: "success"
  
  - name: "Handle malformed log entries"
    description: "Test handling of malformed log entries"
    input:
      log_file: "examples/malformed.log"
      format: "json"
    expected_output:
      entries_parsed: 800
      parsing_errors: 200
      status: "partial_success"
  
  - name: "Process empty log file"
    description: "Test handling of empty log file"
    input:
      log_file: "examples/empty.log"
    expected_output:
      entries_parsed: 0
      status: "success"
      message: "No log entries found"
```

### 2. Integration Testing

```bash
# The wizard runs these tests automatically
qodo agent_wizard --agent_name=test-agent --skip_validation=false
```

**Test Execution Output:**
```
🧪 Running validation tests...

✅ Test 1: Parse standard log file
   - Entries parsed: 1000 ✓
   - Errors found: 5 ✓
   - Status: success ✓

✅ Test 2: Handle malformed log entries  
   - Entries parsed: 800 ✓
   - Parsing errors: 200 ✓
   - Status: partial_success ✓

✅ Test 3: Process empty log file
   - Entries parsed: 0 ✓
   - Status: success ✓

🎉 All tests passed! Your agent is ready to use.
```

## CI/CD Integration Examples

### 1. GitHub Actions Integration

The wizard generates CI/CD examples:

```yaml
# .github/workflows/custom-agent.yml
name: Custom Agent Workflow
on:
  push:
    branches: [main, develop]
  pull_request:
    branches: [main]

jobs:
  run-agent:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout code
        uses: actions/checkout@v3
      
      - name: Setup Qodo CLI
        run: npm install -g @qodo/gen
      
      - name: Run Custom Agent
        run: |
          qodo your-agent-name \
            --input="${{ github.workspace }}/data" \
            --output="${{ github.workspace }}/results" \
            --format=json
      
      - name: Upload Results
        uses: actions/upload-artifact@v3
        with:
          name: agent-results
          path: results/
```

### 2. Jenkins Pipeline

```groovy
// Generated Jenkinsfile
pipeline {
    agent any
    
    parameters {
        choice(
            name: 'ENVIRONMENT',
            choices: ['dev', 'staging', 'prod'],
            description: 'Target environment'
        )
        booleanParam(
            name: 'SKIP_VALIDATION',
            defaultValue: false,
            description: 'Skip validation steps'
        )
    }
    
    stages {
        stage('Setup') {
            steps {
                sh 'npm install -g @qodo/gen'
            }
        }
        
        stage('Run Agent') {
            steps {
                script {
                    def skipValidation = params.SKIP_VALIDATION ? '--skip_validation=true' : ''
                    sh """
                        qodo your-agent-name \\
                            --environment=${params.ENVIRONMENT} \\
                            ${skipValidation} \\
                            --output=results.json
                    """
                }
            }
        }
        
        stage('Process Results') {
            steps {
                archiveArtifacts artifacts: 'results.json', fingerprint: true
                publishHTML([
                    allowMissing: false,
                    alwaysLinkToLastBuild: true,
                    keepAll: true,
                    reportDir: 'reports',
                    reportFiles: 'index.html',
                    reportName: 'Agent Report'
                ])
            }
        }
    }
    
    post {
        always {
            cleanWs()
        }
        failure {
            emailext (
                subject: "Agent Execution Failed: ${env.JOB_NAME} - ${env.BUILD_NUMBER}",
                body: "The agent execution failed. Please check the build logs.",
                to: "${env.CHANGE_AUTHOR_EMAIL}"
            )
        }
    }
}
```

## Troubleshooting Examples

### 1. Debug Mode Usage

```bash
# Run with debug logging
qodo agent_wizard --agent_name=debug-test --log=debug.log

# Check the debug log
cat debug.log
```

### 2. Validation Failures

```bash
# Skip validation if tests are failing
qodo agent_wizard --agent_name=quick-test --skip_validation=true

# Run validation separately later
qodo quick-test --validate-only
```

### 3. MCP Server Issues

```bash
# Test MCP server connectivity
uvx mcp-shell-server --test

# Check MCP server logs
qodo agent_wizard --mcp-debug=true
```

## Best Practices from Examples

### 1. Naming Conventions
- Use kebab-case for agent names: `log-analyzer`, `api-tester`
- Be descriptive but concise: `security-scanner` not `scan`
- Avoid generic names: `data-processor` not `processor`

### 2. Configuration Design
- Always provide sensible defaults
- Make critical parameters required
- Use clear, descriptive parameter names
- Include comprehensive help text

### 3. Error Handling
- Plan for common failure scenarios
- Provide meaningful error messages
- Include recovery suggestions
- Log errors appropriately

### 4. Testing Strategy
- Test happy path scenarios
- Include edge cases and error conditions
- Use realistic test data
- Validate all output formats

These examples demonstrate the flexibility and power of the Agent Wizard in creating various types of agents for different use cases.