# Ugly Stick Agent - Test Scenarios

This document outlines comprehensive test scenarios to validate the Ugly Stick Agent's functionality.

## Test Environment Setup

### Prerequisites
- Git repository with source code
- Multiple programming languages (Python, JavaScript, etc.)
- Clean working directory
- Qodo CLI installed and configured

### Test Project Structure
```
test-project/
├── backend/
│   ├── api/
│   │   ├── auth.py          # Authentication logic
│   │   ├── users.py         # User management
│   │   └── orders.py        # Order processing
│   ├── models/
│   │   ├── user.py          # User model
│   │   └── product.py       # Product model
│   ├── services/
│   │   ├── email.py         # Email service
│   │   └── payment.py       # Payment processing
│   └── utils/
│       ├── helpers.py       # Utility functions
│       └── validators.py    # Input validation
├── frontend/
│   ├── components/
│   │   ├── Login.jsx        # Login component
│   │   ├── Dashboard.jsx    # Dashboard
│   │   └── UserProfile.jsx  # User profile
│   ├── utils/
│   │   ├── api.js           # API client
│   │   ├── validation.js    # Form validation
│   │   └── helpers.js       # Utility functions
│   └── services/
│       ├── auth.js          # Authentication service
│       └── data.js          # Data management
├── tests/                   # Should be skipped
│   ├── test_auth.py
│   └── test_api.js
├── config/                  # Should be skipped
│   ├── settings.py
│   └── webpack.config.js
└── docs/                    # Should be skipped
    └── README.md
```

## Test Scenarios

### 1. Basic Functionality Tests

#### Test 1.1: Default Execution
```bash
# Test basic execution with default parameters
qodo ugly_stick

# Expected Results:
# ✅ Creates new Git branch with timestamp
# ✅ Selects 5-10 meaningful source files
# ✅ Introduces mixed difficulty issues
# ✅ Generates comprehensive report
# ✅ Preserves original code on main branch
```

**Validation Criteria:**
- [ ] New branch created successfully
- [ ] 5-10 files modified (default max_files=10)
- [ ] Issues span all categories (security, performance, bugs, quality, style)
- [ ] Report file generated with detailed breakdown
- [ ] Original files unchanged on main branch

#### Test 1.2: Custom Parameters
```bash
# Test with custom parameters
qodo ugly_stick \
  --branch_name=test-custom-branch \
  --max_files=5 \
  --issue_density=high \
  --difficulty_mix=easy

# Expected Results:
# ✅ Uses specified branch name
# ✅ Modifies exactly 5 files
# ✅ High density of issues per file
# ✅ Issues are easy to spot
```

**Validation Criteria:**
- [ ] Branch name matches specification
- [ ] Exactly 5 files modified
- [ ] High number of issues per file (8+ issues per file)
- [ ] Issues are obvious and easy to identify

#### Test 1.3: Dry Run Mode
```bash
# Test dry run functionality
qodo ugly_stick --dry_run=true --max_files=3

# Expected Results:
# ✅ No files actually modified
# ✅ Shows preview of what would be changed
# ✅ No Git branch created
# ✅ Provides detailed analysis
```

**Validation Criteria:**
- [ ] No files modified in working directory
- [ ] No new Git branches created
- [ ] Preview output shows selected files
- [ ] Estimated issue counts provided

### 2. File Selection Tests

#### Test 2.1: Meaningful File Selection
```bash
# Test that agent selects appropriate files
qodo ugly_stick --dry_run=true

# Expected Results:
# ✅ Selects source files from backend/ and frontend/
# ✅ Skips test files, config files, documentation
# ✅ Prioritizes files with substantial code content
# ✅ Handles multiple programming languages
```

**Validation Criteria:**
- [ ] No files from tests/ directory selected
- [ ] No files from config/ directory selected
- [ ] No files from docs/ directory selected
- [ ] Source files from backend/ and frontend/ selected
- [ ] Files with more code content prioritized

#### Test 2.2: Language Detection
```bash
# Test multi-language support
qodo ugly_stick --max_files=6 --dry_run=true

# Expected Results:
# ✅ Detects Python files (.py)
# ✅ Detects JavaScript/JSX files (.js, .jsx)
# ✅ Applies language-appropriate transformations
# ✅ Reports detected languages
```

**Validation Criteria:**
- [ ] Python files identified correctly
- [ ] JavaScript/JSX files identified correctly
- [ ] Language-specific issue patterns applied
- [ ] Report shows detected languages

#### Test 2.3: Project Path Specification
```bash
# Test targeting specific directories
qodo ugly_stick --project_path=./backend --max_files=3
qodo ugly_stick --project_path=./frontend --max_files=3

# Expected Results:
# ✅ First run only modifies backend files
# ✅ Second run only modifies frontend files
# ✅ Respects directory boundaries
```

**Validation Criteria:**
- [ ] Backend run only touches Python files
- [ ] Frontend run only touches JavaScript/JSX files
- [ ] No cross-directory modifications

### 3. Issue Introduction Tests

#### Test 3.1: Security Vulnerabilities
```bash
# Test security issue introduction
qodo ugly_stick \
  --issue_types='["security"]' \
  --max_files=3 \
  --branch_name=test-security

# Expected Results:
# ✅ Introduces SQL injection opportunities
# ✅ Removes input sanitization
# ✅ Adds hardcoded credentials
# ✅ Creates XSS vulnerabilities
```

**Validation Criteria:**
- [ ] SQL queries use string formatting instead of parameters
- [ ] Input validation removed or weakened
- [ ] Hardcoded API keys or passwords added
- [ ] User input directly inserted into HTML/responses

#### Test 3.2: Performance Issues
```bash
# Test performance issue introduction
qodo ugly_stick \
  --issue_types='["performance"]' \
  --max_files=3 \
  --branch_name=test-performance

# Expected Results:
# ✅ Creates inefficient loops
# ✅ Introduces memory leaks
# ✅ Adds blocking operations
# ✅ Creates N+1 query problems
```

**Validation Criteria:**
- [ ] Nested loops where single loop would suffice
- [ ] Resources opened but not closed
- [ ] Synchronous operations in async contexts
- [ ] Database queries inside loops

#### Test 3.3: Logic Bugs
```bash
# Test logic bug introduction
qodo ugly_stick \
  --issue_types='["bugs"]' \
  --max_files=3 \
  --branch_name=test-bugs

# Expected Results:
# ✅ Off-by-one errors in loops
# ✅ Null pointer opportunities
# ✅ Race conditions
# ✅ Incorrect conditional logic
```

**Validation Criteria:**
- [ ] Loop bounds modified to cause index errors
- [ ] Null checks removed
- [ ] Shared variables accessed without synchronization
- [ ] Comparison operators changed (== vs ===, < vs <=)

#### Test 3.4: Code Quality Issues
```bash
# Test code quality issue introduction
qodo ugly_stick \
  --issue_types='["quality"]' \
  --max_files=3 \
  --branch_name=test-quality

# Expected Results:
# ✅ Duplicate code blocks
# ✅ Long methods/functions
# ✅ Poor variable naming
# ✅ Magic numbers
```

**Validation Criteria:**
- [ ] Code blocks duplicated instead of extracted to functions
- [ ] Functions combined into overly long methods
- [ ] Descriptive variable names changed to generic ones
- [ ] Constants replaced with hardcoded values

#### Test 3.5: Style Violations
```bash
# Test style violation introduction
qodo ugly_stick \
  --issue_types='["style"]' \
  --max_files=3 \
  --branch_name=test-style

# Expected Results:
# ✅ Inconsistent indentation
# ✅ Missing documentation
# ✅ Complex expressions
# ✅ Naming inconsistencies
```

**Validation Criteria:**
- [ ] Mixed tabs and spaces for indentation
- [ ] Docstrings and comments removed
- [ ] Simple expressions made unnecessarily complex
- [ ] Inconsistent naming conventions (camelCase vs snake_case)

### 4. Difficulty Level Tests

#### Test 4.1: Easy Issues
```bash
# Test easy-to-spot issues
qodo ugly_stick \
  --difficulty_mix=easy \
  --max_files=3 \
  --branch_name=test-easy

# Expected Results:
# ✅ Obvious style violations
# ✅ Clear logic errors
# ✅ Blatant security issues
# ✅ Simple performance problems
```

**Validation Criteria:**
- [ ] Issues are immediately obvious upon inspection
- [ ] Style violations clearly visible
- [ ] Logic errors cause obvious problems
- [ ] Security issues are blatant (hardcoded passwords, etc.)

#### Test 4.2: Subtle Issues
```bash
# Test subtle, hard-to-spot issues
qodo ugly_stick \
  --difficulty_mix=subtle \
  --max_files=3 \
  --branch_name=test-subtle

# Expected Results:
# ✅ Subtle race conditions
# ✅ Edge case handling removed
# ✅ Minor security vulnerabilities
# ✅ Performance issues in edge cases
```

**Validation Criteria:**
- [ ] Issues require careful code review to spot
- [ ] Race conditions only occur under specific timing
- [ ] Security issues are not immediately obvious
- [ ] Performance problems only manifest with large data sets

#### Test 4.3: Mixed Difficulty
```bash
# Test mixed difficulty levels
qodo ugly_stick \
  --difficulty_mix=mixed \
  --max_files=5 \
  --branch_name=test-mixed

# Expected Results:
# ✅ Combination of easy and subtle issues
# ✅ Varied complexity across files
# ✅ Good training scenario balance
```

**Validation Criteria:**
- [ ] Some issues are immediately obvious
- [ ] Some issues require careful analysis
- [ ] Good distribution of difficulty levels
- [ ] Realistic mix for training purposes

### 5. Git Integration Tests

#### Test 5.1: Branch Creation
```bash
# Test Git branch functionality
qodo ugly_stick --branch_name=test-git-integration

# Expected Results:
# ✅ New branch created with specified name
# ✅ All changes committed to new branch
# ✅ Original branch unchanged
# ✅ Proper commit messages
```

**Validation Criteria:**
- [ ] `git branch` shows new branch exists
- [ ] `git checkout test-git-integration && git log` shows commits
- [ ] `git checkout main && git status` shows clean working directory
- [ ] Commit messages describe changes made

#### Test 5.2: Branch Isolation
```bash
# Test that changes are isolated to new branch
qodo ugly_stick --branch_name=test-isolation
git checkout main

# Expected Results:
# ✅ Main branch files are unchanged
# ✅ New branch contains all modifications
# ✅ Can switch between branches safely
```

**Validation Criteria:**
- [ ] Files on main branch match original state
- [ ] Files on test branch contain introduced issues
- [ ] `git diff main test-isolation` shows all changes

#### Test 5.3: Multiple Branch Creation
```bash
# Test creating multiple training branches
qodo ugly_stick --branch_name=scenario-1 --max_files=3
qodo ugly_stick --branch_name=scenario-2 --max_files=3
qodo ugly_stick --branch_name=scenario-3 --max_files=3

# Expected Results:
# ✅ Three separate branches created
# ✅ Each branch has different issues
# ✅ No interference between branches
```

**Validation Criteria:**
- [ ] `git branch` shows all three branches
- [ ] Each branch has different file modifications
- [ ] Issues introduced are different across branches

### 6. Reporting Tests

#### Test 6.1: Report Generation
```bash
# Test comprehensive report generation
qodo ugly_stick \
  --create_report=true \
  --max_files=5 \
  --branch_name=test-reporting

# Expected Results:
# ✅ Detailed report file created
# ✅ Issue breakdown by category
# ✅ File-by-file analysis
# ✅ Solution key provided
```

**Validation Criteria:**
- [ ] Report file exists and is readable
- [ ] Contains summary statistics
- [ ] Lists all issues with locations
- [ ] Provides educational explanations

#### Test 6.2: Report Content Validation
```bash
# Validate report content accuracy
qodo ugly_stick --max_files=3 --branch_name=test-report-content
cat ugly-stick-report.md

# Expected Results:
# ✅ Issue counts match actual changes
# ✅ File locations are accurate
# ✅ Issue descriptions are helpful
# ✅ Solution explanations are educational
```

**Validation Criteria:**
- [ ] Reported issue count matches actual issues found
- [ ] Line numbers in report match actual changes
- [ ] Issue descriptions accurately describe problems
- [ ] Solutions provide learning value

### 7. Error Handling Tests

#### Test 7.1: Non-Git Directory
```bash
# Test behavior in non-Git directory
cd /tmp
mkdir test-no-git
cd test-no-git
echo "print('hello')" > test.py
qodo ugly_stick

# Expected Results:
# ❌ Should fail gracefully with clear error message
# ❌ Should not modify files without Git repository
```

**Validation Criteria:**
- [ ] Clear error message about missing Git repository
- [ ] No files modified
- [ ] Graceful failure without crashes

#### Test 7.2: No Source Files
```bash
# Test behavior with no suitable source files
mkdir empty-project
cd empty-project
git init
touch README.md config.yml
qodo ugly_stick

# Expected Results:
# ❌ Should report no suitable files found
# ❌ Should not create branch or make changes
```

**Validation Criteria:**
- [ ] Clear message about no suitable files
- [ ] No Git branch created
- [ ] No modifications attempted

#### Test 7.3: Dirty Working Directory
```bash
# Test behavior with uncommitted changes
echo "# Modified" >> README.md
qodo ugly_stick

# Expected Results:
# ❌ Should warn about uncommitted changes
# ❌ Should either fail or handle gracefully
```

**Validation Criteria:**
- [ ] Detects uncommitted changes
- [ ] Provides clear guidance on how to proceed
- [ ] Doesn't lose user's work

### 8. Performance Tests

#### Test 8.1: Large Project Handling
```bash
# Test with large number of files
qodo ugly_stick --max_files=20 --dry_run=true

# Expected Results:
# ✅ Handles large projects efficiently
# ✅ Reasonable execution time
# ✅ Memory usage stays reasonable
```

**Validation Criteria:**
- [ ] Completes within reasonable time (< 30 seconds for dry run)
- [ ] Memory usage doesn't spike excessively
- [ ] File selection algorithm scales well

#### Test 8.2: Large File Handling
```bash
# Test with very large source files
# Create a large Python file for testing
python -c "
with open('large_file.py', 'w') as f:
    f.write('# Large file test\\n')
    for i in range(1000):
        f.write(f'def function_{i}():\\n    return {i}\\n\\n')
"
qodo ugly_stick --max_files=1

# Expected Results:
# ✅ Handles large files without issues
# ✅ Introduces appropriate number of issues
# ✅ Maintains reasonable performance
```

**Validation Criteria:**
- [ ] Successfully processes large files
- [ ] Issues distributed throughout the file
- [ ] No performance degradation

### 9. Integration Tests

#### Test 9.1: CI/CD Pipeline Integration
```bash
# Test in automated environment
export CI=true
qodo ugly_stick \
  --branch_name=ci-test-$(date +%s) \
  --max_files=5 \
  --create_report=true

# Expected Results:
# ✅ Works in non-interactive environment
# ✅ Produces machine-readable output
# ✅ Exit codes indicate success/failure
```

**Validation Criteria:**
- [ ] Runs successfully in CI environment
- [ ] Produces structured output
- [ ] Proper exit codes for automation

#### Test 9.2: Static Analysis Tool Integration
```bash
# Test integration with linting tools
qodo ugly_stick --branch_name=lint-test --issue_types='["style", "quality"]'
git checkout lint-test

# Run various linting tools
python -m flake8 . || echo "Flake8 found issues (expected)"
npm run eslint . || echo "ESLint found issues (expected)"

# Expected Results:
# ✅ Linting tools detect introduced issues
# ✅ Issues are realistic and detectable
# ✅ Tools don't crash on introduced problems
```

**Validation Criteria:**
- [ ] Static analysis tools detect introduced issues
- [ ] No false positives on legitimate code patterns
- [ ] Tools handle introduced issues gracefully

## Test Execution Checklist

### Pre-Test Setup
- [ ] Clean Git repository with committed changes
- [ ] Multiple source files in different languages
- [ ] Qodo CLI installed and configured
- [ ] Test project structure created

### Test Execution
- [ ] Run each test scenario
- [ ] Validate expected results
- [ ] Check error conditions
- [ ] Verify Git branch isolation
- [ ] Validate report accuracy

### Post-Test Cleanup
- [ ] Delete test branches: `git branch | grep test- | xargs git branch -D`
- [ ] Reset to clean state: `git checkout main && git clean -fd`
- [ ] Remove test files and directories

### Success Criteria
- [ ] All basic functionality tests pass
- [ ] File selection works correctly
- [ ] All issue types are introduced properly
- [ ] Difficulty levels work as expected
- [ ] Git integration is solid
- [ ] Reports are accurate and helpful
- [ ] Error handling is graceful
- [ ] Performance is acceptable
- [ ] Integration scenarios work

## Automated Test Script

```bash
#!/bin/bash
# run-ugly-stick-tests.sh

set -e

echo "🧪 Starting Ugly Stick Agent Test Suite"

# Test 1: Basic functionality
echo "Test 1: Basic functionality"
qodo ugly_stick --dry_run=true --max_files=3
echo "✅ Basic functionality test passed"

# Test 2: Security issues
echo "Test 2: Security issue introduction"
qodo ugly_stick --issue_types='["security"]' --max_files=2 --branch_name=test-security
git checkout test-security
# Verify security issues were introduced
git diff main | grep -q "hardcoded\|injection\|XSS" && echo "✅ Security issues introduced"
git checkout main

# Test 3: Performance issues
echo "Test 3: Performance issue introduction"
qodo ugly_stick --issue_types='["performance"]' --max_files=2 --branch_name=test-performance
git checkout test-performance
# Verify performance issues were introduced
git diff main | grep -q "loop\|inefficient\|blocking" && echo "✅ Performance issues introduced"
git checkout main

# Test 4: Report generation
echo "Test 4: Report generation"
qodo ugly_stick --max_files=2 --branch_name=test-report --create_report=true
test -f ugly-stick-report.md && echo "✅ Report generated successfully"

# Cleanup
echo "🧹 Cleaning up test branches"
git branch | grep test- | xargs -r git branch -D

echo "🎉 All tests passed!"
```

This comprehensive test suite ensures the Ugly Stick Agent works correctly across all scenarios and edge cases.