# Agent Wizard

An intelligent wizard agent that guides users through creating custom agents with proper configuration, MCP integration, and validation.

## Overview

The Agent Wizard is your personal assistant for creating new agents. It asks thoughtful questions to understand your needs, identifies required tools and integrations, generates proper configuration files, and validates the created agent with real examples.

## Features

- 🧙‍♂️ **Interactive Wizard**: Guides you through agent creation with smart questions
- 🔍 **Requirements Discovery**: Identifies needed tools, MCPs, and integrations
- ⚙️ **Configuration Generation**: Creates both TOML and YAML configurations
- 🔌 **MCP Integration**: Automatically configures required MCP servers
- ✅ **Validation & Testing**: Runs example scenarios to verify agent functionality
- 📚 **Documentation**: Generates comprehensive README with usage examples
- 🚀 **CI/CD Ready**: Includes integration examples for various platforms

## Quick Start

### Basic Usage

```bash
# Start the interactive wizard
qodo agent_wizard

# Create agent with specific name
qodo agent_wizard --agent_name=my-custom-agent

# Non-interactive mode with purpose
qodo agent_wizard --agent_name=data-processor --agent_purpose="Process CSV files and generate reports" --interactive=false
```

### Advanced Configuration

```bash
# Generate only TOML configuration
qodo agent_wizard --output_format=toml --skip_validation=false

# Quick creation without validation
qodo agent_wizard --agent_name=quick-agent --skip_validation=true
```

## Configuration Options

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `agent_name` | string | - | Name for the new agent (prompted if not provided) |
| `agent_purpose` | string | - | Brief description of the agent's purpose |
| `skip_validation` | boolean | `false` | Skip the validation/testing phase |
| `output_format` | string | `both` | Configuration format (toml, yaml, or both) |
| `interactive` | boolean | `true` | Run in interactive mode with questions |

## How It Works

### 1. Discovery Phase
The wizard asks comprehensive questions to understand your needs:
- Primary purpose and goals
- Specific tasks to perform
- Input and output requirements
- Tool and service integrations
- Success criteria and constraints

### 2. Requirements Analysis
Based on your answers, it identifies:
- Required MCP servers (shell, web, database, etc.)
- Necessary tools and integrations
- Authentication requirements
- Configuration parameters

### 3. Agent Generation
Creates a complete agent structure:
- `agent.toml` - Primary configuration
- `agent.yaml` - Alternative format
- `README.md` - Comprehensive documentation
- `examples/` - Usage examples and test scenarios

### 4. Validation & Testing
Runs the created agent with example scenarios:
- Creates realistic test cases
- Executes the agent with sample data
- Verifies expected behavior
- Documents any issues found

## Example Walkthrough

Here's what a typical session looks like:

```
🧙‍♂️ Welcome to the Agent Wizard!

Let's create your custom agent step by step.

? What would you like to name your agent? 
> log-analyzer

? What is the primary purpose of your agent?
> Analyze application log files to identify errors, patterns, and generate reports

? What types of inputs will your agent receive?
> Log files (text files, JSON logs, CSV format)

? What outputs should it produce?
> Summary reports, error statistics, trend analysis, alerts for critical issues

? What tools or services does it need to integrate with?
> File system for reading logs, possibly databases for storing results, email for alerts

? Will this agent be used in CI/CD pipelines?
> Yes, we want to run it after deployments to check for issues

✨ Based on your answers, I'll configure:
- MCP servers: shell, filesystem, database, email
- Tools: file processing, data analysis, reporting
- Output schema: structured reports with metrics
- CI/CD integration examples

Creating your agent...
✅ Generated agent.toml
✅ Generated agent.yaml  
✅ Generated README.md
✅ Created example scenarios

Testing your agent...
✅ Test 1: Process sample log file - PASSED
✅ Test 2: Generate error report - PASSED
✅ Test 3: Identify critical issues - PASSED

🎉 Your agent 'log-analyzer' is ready!

Usage examples:
- qodo log-analyzer --log_file=app.log
- qodo log-analyzer --directory=logs/ --output_format=json
- qodo log-analyzer --alert_threshold=critical --email_alerts=true
```

## Generated Agent Structure

When you create an agent, the wizard generates:

```
agents/your-agent-name/
├── agent.toml              # Primary configuration
├── agent.yaml              # Alternative YAML format
├── README.md               # Comprehensive documentation
└── examples/
    ├── usage.md            # Usage examples
    ├── test-scenarios.md   # Test cases
    └── integration/        # CI/CD examples
        ├── github-actions.yml
        ├── gitlab-ci.yml
        └── jenkins-pipeline.groovy
```

## MCP Server Integration

The wizard automatically configures MCP servers based on your needs:

### Common MCP Servers
- **shell** - Command execution, system operations
- **filesystem** - File and directory operations
- **web** - HTTP requests, API calls
- **database** - SQL operations, data storage
- **email** - Email notifications and alerts
- **git** - Version control operations
- **docker** - Container operations
- **aws/gcp/azure** - Cloud service integrations

### Custom MCP Configuration
The wizard can also help you configure custom MCP servers:

```toml
[mcpServers.custom_api]
command = "node"
args = ["custom-mcp-server.js"]
env = { API_KEY = "${API_KEY}" }
```

## Agent Types & Templates

The wizard recognizes common agent patterns and provides optimized templates:

### 1. **Code Analysis Agents**
- Static code analysis
- Security vulnerability scanning
- Code quality metrics
- Dependency analysis

### 2. **Data Processing Agents**
- File format conversion
- Data validation and cleaning
- Report generation
- Batch processing

### 3. **Integration Agents**
- API orchestration
- Service monitoring
- Deployment automation
- Notification systems

### 4. **Testing Agents**
- Test generation
- Test execution
- Coverage analysis
- Performance testing

### 5. **Documentation Agents**
- API documentation
- Code documentation
- User guide generation
- Change log creation

## Validation & Testing

The wizard creates comprehensive test scenarios:

### Test Types
1. **Unit Tests** - Individual function testing
2. **Integration Tests** - End-to-end workflows
3. **Error Handling** - Edge cases and failures
4. **Performance Tests** - Load and stress testing

### Example Test Scenario
```yaml
test_scenarios:
  - name: "Basic functionality"
    input: { file: "sample.log", format: "json" }
    expected_output: { errors: 5, warnings: 12, status: "success" }
  
  - name: "Error handling"
    input: { file: "nonexistent.log" }
    expected_error: "File not found"
  
  - name: "Large file processing"
    input: { file: "large.log", size: "100MB" }
    timeout: 300
    expected_output: { processed: true, duration: "<60s" }
```

## Integration Examples

### GitHub Actions
```yaml
name: Custom Agent
on: [push, pull_request]

jobs:
  run-agent:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - name: Setup Qodo CLI
        run: npm install -g @qodo/gen
      - name: Run Custom Agent
        run: qodo your-agent-name --input=${{ github.workspace }}/data
```

### GitLab CI
```yaml
custom-agent:
  stage: analysis
  script:
    - qodo your-agent-name --config=production
  artifacts:
    reports:
      junit: agent-results.xml
```

### Jenkins Pipeline
```groovy
pipeline {
    agent any
    stages {
        stage('Run Agent') {
            steps {
                sh 'qodo your-agent-name --environment=${ENVIRONMENT}'
            }
        }
    }
}
```

## Best Practices

### 1. Agent Design
- **Single Responsibility**: Each agent should have one clear purpose
- **Clear Interfaces**: Define inputs and outputs explicitly
- **Error Handling**: Plan for edge cases and failures
- **Documentation**: Provide comprehensive usage examples

### 2. Configuration
- **Parameterization**: Make agents configurable for different environments
- **Validation**: Include input validation and type checking
- **Defaults**: Provide sensible default values
- **Schema**: Define clear output schemas for integration

### 3. Testing
- **Comprehensive Coverage**: Test all major code paths
- **Real Data**: Use realistic test scenarios
- **Performance**: Include performance benchmarks
- **Continuous Testing**: Integrate with CI/CD pipelines

### 4. MCP Integration
- **Minimal Dependencies**: Only include necessary MCP servers
- **Security**: Use environment variables for sensitive data
- **Error Handling**: Handle MCP server failures gracefully
- **Documentation**: Document MCP requirements clearly

## Troubleshooting

### Common Issues

**Wizard doesn't start:**
- Ensure Qodo CLI is properly installed
- Check that you're in the correct directory
- Verify agent-wizard is available: `qodo --list-agents`

**MCP server configuration fails:**
- Check MCP server installation: `uvx mcp-shell-server --version`
- Verify environment variables are set
- Review MCP server logs for errors

**Generated agent doesn't work:**
- Review the generated configuration files
- Check that all required dependencies are installed
- Run the validation tests manually
- Review the agent logs for error messages

**Test scenarios fail:**
- Verify test data is available and accessible
- Check file permissions and paths
- Review expected vs actual outputs
- Update test scenarios if requirements changed

### Debug Mode

```bash
# Run wizard with detailed logging
qodo agent_wizard --log=debug.log

# Save output for analysis
qodo -q agent_wizard > wizard-output.json 2> debug.log
```

## Advanced Usage

### Custom Templates
You can create custom agent templates by modifying the wizard's template directory:

```bash
# Copy existing template
cp -r agents/agent-wizard/templates/basic agents/agent-wizard/templates/my-template

# Modify template files
# Use the custom template
qodo agent_wizard --template=my-template
```

### Batch Agent Creation
Create multiple agents from a configuration file:

```yaml
# agents-config.yaml
agents:
  - name: "log-analyzer"
    purpose: "Analyze application logs"
    tools: ["filesystem", "shell"]
  - name: "api-tester"
    purpose: "Test REST APIs"
    tools: ["web", "filesystem"]
```

```bash
qodo agent_wizard --batch=agents-config.yaml
```

## Contributing

Found an issue or want to improve the Agent Wizard? Please see our [Contributing Guide](../../CONTRIBUTING.md).

### Reporting Issues
- Include the wizard session output
- Provide the generated agent configuration
- Include any error messages or unexpected behavior
- Describe the expected vs actual results

### Suggesting Improvements
- Describe the enhancement and its benefits
- Provide examples of the desired behavior
- Consider backward compatibility with existing agents
- Include test scenarios for new features

## License

This agent is part of the Qodo Agent Reference Implementations and is licensed under the MIT License.