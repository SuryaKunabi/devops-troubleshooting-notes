
# GitHub Actions Troubleshooting – Real Issues & Solutions

This document records issues encountered while configuring GitHub Actions with a self-hosted runner on an AWS EC2 Ubuntu instance.

## 1. GitHub Actions – Pytest Collected 0 Items

**Problem:**

The GitHub Actions workflow executed Python tests, but pytest did not collect any test cases.

**Actual Output:**

```text
collected 0 items

Error: Process completed with exit code 5.
```

**Environment:**

- AWS EC2
- Ubuntu
- GitHub Actions
- Self-hosted Runner
- Python
- Pytest

**Root Cause:**

Pytest did not discover any test functions or test files matching its discovery rules.

**Solution:**

1. Verify the test file name starts with `test_` or ends with `_test.py`.
2. Verify test functions start with `test_`.
3. Check that the test directory exists.
4. Confirm the workflow executes pytest from the correct working directory.

**Example:**

```python
# test_app.py

def test_addition():
    assert 2 + 3 == 5
```

Run:

```bash
pytest -v
```

**Learning:** Pytest exit code 5 indicates that no tests were collected. A successful workflow requires valid test discovery and execution.

---

## 2. GitHub Actions – Self-Hosted Runner on EC2

**Problem:**

A self-hosted GitHub Actions runner was configured on an AWS EC2 Ubuntu instance to execute Python tests.

**Troubleshooting Commands:**

```bash
# Check runner directory
ls -la

# Check runner status
sudo ./svc.sh status

# Start runner interactively
./run.sh
```

**Verification:**

Check the repository:

GitHub → Settings → Actions → Runners

Confirm that the runner is online and available to execute jobs.

**Learning:** Self-hosted runners allow GitHub Actions workflows to execute on your own infrastructure.

---

## Key Learnings

- Configure GitHub Actions workflows.
- Set up self-hosted runners on EC2.
- Execute Python tests through CI/CD.
- Understand pytest test discovery.
- Troubleshoot workflow exit codes.
