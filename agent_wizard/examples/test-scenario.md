# Agent Wizard Test Scenarios

This document outlines test scenarios to validate the Agent Wizard functionality.

## Test Scenario 1: Basic Agent Creation

### Objective
Verify that the wizard can create a simple file processing agent with basic configuration.

### Test Steps
1. Run the wizard: `qodo agent_wizard --agent_name=test-file-processor`
2. Answer the discovery questions:
   - **Purpose**: Process text files and count word frequencies
   - **Inputs**: Text files, directories containing text files
   - **Outputs**: Word frequency reports in JSON format
   - **Tools**: File system operations
   - **Success**: Files processed without errors, valid JSON output

### Expected Results
- Agent directory created: `agents/test-file-processor/`
- Files generated:
  - `agent.toml` with proper configuration
  - `agent.yaml` with equivalent configuration
  - `README.md` with usage examples
  - `examples/usage.md` with test scenarios
- Validation tests pass
- Agent can be executed successfully

### Validation Commands
```bash
# Test the created agent
qodo test-file-processor --input_file=sample.txt --output_format=json

# Verify output structure
cat output.json | jq '.word_frequencies | length'
```

## Test Scenario 2: Complex Integration Agent

### Objective
Create an agent that requires multiple MCP servers and complex configuration.

### Test Steps
1. Run: `qodo agent_wizard --agent_name=test-api-monitor`
2. Specify requirements:
   - **Purpose**: Monitor multiple APIs and send alerts
   - **Inputs**: API endpoint configurations, alert thresholds
   - **Outputs**: Health reports, alert notifications
   - **Tools**: HTTP client, email notifications, database storage
   - **Integrations**: REST APIs, SMTP server, PostgreSQL database

### Expected Results
- MCP servers configured: web, email, database
- Complex argument schema with validation
- Proper error handling configuration
- CI/CD integration examples included

## Test Scenario 3: Non-Interactive Mode

### Objective
Verify wizard works in non-interactive mode with minimal parameters.

### Test Steps
```bash
qodo agent_wizard \
  --agent_name=test-batch-processor \
  --agent_purpose="Process batch jobs from queue" \
  --interactive=false \
  --output_format=toml
```

### Expected Results
- Agent created with default configurations
- Only TOML file generated (not YAML)
- Basic functionality working
- Minimal but functional configuration

## Test Scenario 4: Validation Testing

### Objective
Ensure the validation phase properly tests created agents.

### Test Steps
1. Create agent with validation enabled
2. Verify test scenarios are generated
3. Check that tests actually run
4. Confirm test results are reported

### Expected Test Scenarios
- Basic functionality test
- Error handling test
- Edge case handling
- Performance test (if applicable)

## Test Scenario 5: Error Handling

### Objective
Test wizard behavior with invalid inputs and error conditions.

### Test Cases
1. **Invalid agent name**: Special characters, spaces
2. **Missing dependencies**: MCP servers not available
3. **Permission issues**: Cannot create directories
4. **Configuration conflicts**: Conflicting parameters

### Expected Behavior
- Clear error messages
- Graceful failure handling
- Helpful suggestions for resolution
- No partial agent creation on failure

## Automated Test Suite

### Setup
```bash
# Create test directory
mkdir -p test-agents
cd test-agents

# Set up test environment
export QODO_TEST_MODE=true
```

### Test Runner Script
```bash
#!/bin/bash
# run-wizard-tests.sh

set -e

echo "🧪 Running Agent Wizard Test Suite"

# Test 1: Basic agent creation
echo "Test 1: Basic agent creation"
qodo agent_wizard \
  --agent_name=test-basic \
  --agent_purpose="Basic test agent" \
  --interactive=false \
  --skip_validation=true

if [ -d "agents/test-basic" ]; then
    echo "✅ Test 1 passed"
else
    echo "❌ Test 1 failed"
    exit 1
fi

# Test 2: YAML only output
echo "Test 2: YAML only output"
qodo agent_wizard \
  --agent_name=test-yaml \
  --agent_purpose="YAML test agent" \
  --interactive=false \
  --output_format=yaml \
  --skip_validation=true

if [ -f "agents/test-yaml/agent.yaml" ] && [ ! -f "agents/test-yaml/agent.toml" ]; then
    echo "✅ Test 2 passed"
else
    echo "❌ Test 2 failed"
    exit 1
fi

# Test 3: Validation enabled
echo "Test 3: Validation enabled"
qodo agent_wizard \
  --agent_name=test-validation \
  --agent_purpose="Validation test agent" \
  --interactive=false \
  --skip_validation=false

# Check if validation results are included
if grep -q "validation_results" agents/test-validation/README.md; then
    echo "✅ Test 3 passed"
else
    echo "❌ Test 3 failed"
    exit 1
fi

echo "🎉 All tests passed!"
```

### Performance Test
```bash
#!/bin/bash
# performance-test.sh

echo "⏱️  Performance Test: Creating 10 agents"

start_time=$(date +%s)

for i in {1..10}; do
    qodo agent_wizard \
      --agent_name=perf-test-$i \
      --agent_purpose="Performance test agent $i" \
      --interactive=false \
      --skip_validation=true \
      --quiet=true
done

end_time=$(date +%s)
duration=$((end_time - start_time))

echo "Created 10 agents in ${duration} seconds"
echo "Average: $((duration / 10)) seconds per agent"

if [ $duration -lt 60 ]; then
    echo "✅ Performance test passed"
else
    echo "⚠️  Performance test slow (>60s total)"
fi
```

## Manual Testing Checklist

### Pre-Test Setup
- [ ] Qodo CLI installed and working
- [ ] MCP servers available (shell, web, etc.)
- [ ] Test directory prepared
- [ ] Environment variables set

### Basic Functionality
- [ ] Wizard starts without errors
- [ ] Interactive mode works
- [ ] Non-interactive mode works
- [ ] Agent directory created
- [ ] Configuration files generated
- [ ] README.md created with examples

### Configuration Validation
- [ ] TOML syntax is valid
- [ ] YAML syntax is valid
- [ ] Required fields present
- [ ] Default values applied correctly
- [ ] MCP servers configured properly

### Generated Agent Testing
- [ ] Created agent can be listed: `qodo --list-agents`
- [ ] Agent help works: `qodo agent-name --help`
- [ ] Basic execution works: `qodo agent-name`
- [ ] Arguments are processed correctly
- [ ] Output format matches schema

### Error Handling
- [ ] Invalid agent names rejected
- [ ] Missing dependencies detected
- [ ] Permission errors handled gracefully
- [ ] Partial failures cleaned up

### Documentation Quality
- [ ] README.md is comprehensive
- [ ] Usage examples are clear
- [ ] Integration examples provided
- [ ] Troubleshooting section included

## Continuous Integration

### GitHub Actions Test
```yaml
name: Agent Wizard Tests
on: [push, pull_request]

jobs:
  test-wizard:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - name: Setup Qodo CLI
        run: npm install -g @qodo/gen
      - name: Run Wizard Tests
        run: |
          chmod +x test/run-wizard-tests.sh
          ./test/run-wizard-tests.sh
      - name: Validate Generated Agents
        run: |
          for agent in test-agents/agents/*/; do
            agent_name=$(basename "$agent")
            echo "Testing agent: $agent_name"
            qodo "$agent_name" --help
          done
```

These test scenarios ensure the Agent Wizard works correctly across different use cases and configurations.