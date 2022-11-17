# run-tests

It is a custom Github Action for running tests for CWL workflows.

## Example

```yaml
jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      # setup CWL runner
      - uses: actions/checkout@v2
      - name: Setup python for cwltool
        uses: actions/setup-python@v4
        with:
          python-version: '3.9.x'
      - name: Install cwltool
        run: pip install cwltool
      - uses: actions/setup-node@v3
        with:
          node-version: '18.x'

      - name: Run tests
        id: run-tests
        uses: common-workflow-lab/run-tests@v1
        with:
          test-list: test.yml
          runner: cwltool
          timeout: 30
          result-title: Example test results
     
      - name: Do something with resulted JUnit XML file
        run: |
          echo ${{ steps.run-conformance.outputs.result }}
```

## Input parameters

| Parameters | Required | Default | Description |
|---|---|---|---|
| `test-list` | true | - | file that lists test cases in cwltest format |
| `runner` | true | - | full path to CWL runner to be tested |
| `timeout` | false | 30 | timeout in seconds |
| `skip-python-install` | false | false | skip installing python interpreter |
| `allow-failure` | false | false | whether this action always succeeds even if some tests fail |
| `result-title` | false | `"Test result"` | title for test result |

The `skip-python-install` parameter is useful when CWL runner requires specific version of Python.

## Output parameters

| Parameters | Description |
|---|---|
| `result` | file name that stores the result of tests in JUnit XML format |
