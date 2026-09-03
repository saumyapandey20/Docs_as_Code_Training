# Troubleshooting BMC AMI DevX Total Test

Use this information to diagnose and resolve common problems when creating, executing, or reviewing automated tests in BMC AMI DevX Total Test.

## Before you begin

Before troubleshooting a test failure:

- Record the displayed error message and error code.
- Confirm the installed product version.
- Identify whether the test is **virtualized** or **non-virtualized**.
- Verify that the target mainframe system is available.
- Collect the relevant test execution logs.
- Confirm whether the problem affects one test or multiple tests.

> [!IMPORTANT]
> Back up test definitions and configuration files before changing or deleting them.

## Quick problem identification

Use the following table to identify the recommended troubleshooting action.

| Problem | Possible cause | Recommended action |
|---|---|---|
| Test does not start | Connection or authentication failure | Verify the connection and credentials |
| Test fails unexpectedly | Input or expected result is incorrect | Review the test data |
| Program cannot be found | Incorrect program or load-library information | Verify the program configuration |
| Expected result does not match | Program behavior or data changed | Compare the expected and actual values |
| Test remains in progress | Execution service is unavailable | Check the service and cancel the test if necessary |
| Results do not appear | Test execution did not complete | Refresh the results and review the logs |

---

## Test does not start

### Symptom

The test remains in the **Pending** state or displays the following sample message:

```text
Unable to start the test.
Connection to the target system could not be established.
```

### Possible causes

The problem can occur for one or more of the following reasons:

- The target system is unavailable.
- The connection information is incorrect.
- The user credentials have expired.
- A required service is not running.
- A firewall is blocking the connection.

### Resolution

1. Confirm that the target system is available.

2. Verify the following connection values:

   | Setting | Example value |
   |---|---|
   | Host name | `mainframe.example.com` |
   | Port | `16196` |
   | Protocol | `TLS` |
   | Environment | `TEST` |

3. Test the network connection:

   ```powershell
   Test-NetConnection mainframe.example.com -Port 16196
   ```

4. Verify that your user ID has permission to access the test environment.

5. Re-enter your credentials if they have changed or expired.

6. Run the test again.

> [!WARNING]
> Do not enter passwords, access tokens, or other credentials in test definitions or log files.

<details>
<summary><strong>Show additional connection checks</strong></summary>

If the problem continues, check the following items:

1. Confirm that the host name resolves to the expected IP address.

   ```powershell
   Resolve-DnsName mainframe.example.com
   ```

2. Verify that the required service is running.

3. Confirm that the firewall permits traffic through the configured port.

4. Review the connection log for timeout or authentication errors.

5. Ask the system administrator whether the target environment is undergoing maintenance.

</details>

---

## Expected and actual results do not match

### Symptom

The test runs successfully, but one or more assertions fail.

```text
Assertion failed for field FINAL-AMOUNT.
Expected: 900.00
Actual:   950.00
```

### Possible causes

- The input data changed.
- The expected result is outdated.
- The program logic changed.
- An external dependency returned a different value.
- The test is using data from the wrong environment.

### Resolution

1. Open the failed test result.

2. Compare the **Expected value** with the **Actual value**.

3. Review the input values used by the test:

   ```yaml
   input:
     customerType: GOLD
     orderAmount: 1000

   expected:
     discountPercentage: 10
     finalAmount: 900
   ```

4. Confirm whether the application change was intentional.

5. Take the appropriate action:

   - If the actual result is incorrect, correct the program.
   - If the expected behavior changed, update the test.
   - If the input data is incorrect, correct the test data.

6. Run the test again.

> [!NOTE]
> Do not update an expected result merely to make a failed test pass. First confirm the intended application behavior with the appropriate subject-matter expert.

### Expected result comparison

| Field | Expected | Actual | Status |
|---|---:|---:|---|
| Order amount | `1000.00` | `1000.00` | Passed |
| Discount percentage | `10` | `5` | Failed |
| Final amount | `900.00` | `950.00` | Failed |

---

## Program cannot be found

### Symptom

The test displays the following sample error:

```text
Program CUSTDISC could not be located.
Error code: TT-104
```

### Resolution

Use the following checklist:

- [ ] Verify that the program name is spelled correctly.
- [ ] Confirm that the program exists in the target environment.
- [ ] Verify the load-library configuration.
- [ ] Confirm that the correct environment is selected.
- [ ] Check that your user ID can access the required resources.
- [ ] Rebuild or redeploy the program if necessary.

<details>
<summary><strong>Show sample program configuration</strong></summary>

The following fictional YAML example is provided for Markdown practice. It is not actual product syntax.

```yaml
program:
  name: CUSTDISC
  environment: TEST
  loadLibrary: USER.TEST.LOAD
  language: COBOL
```

Confirm that the values match the program deployed in your environment.

</details>

---

## Test remains in progress

### Symptom

The test remains in the **In progress** state and does not produce a result.

### Resolution

1. Wait a few minutes and refresh the test status.
2. Determine whether other tests are completing.
3. Review the execution-service status.
4. Check the log for timeout messages.
5. Cancel the test if it is no longer processing.
6. Correct the underlying problem and run the test again.

> [!CAUTION]
> Cancelling a non-virtualized test might leave test data in an incomplete state. Verify the affected data before running another the test.

### Test-status meanings

| Status | Meaning |
|---|---|
| **Pending** | The test is waiting to start |
| **In progress** | The test is currently running |
| **Passed** | All required validations succeeded |
| **Failed** | One or more validations failed |
| **Cancelled** | A user or system process stopped the test |

---

## Test passes locally but fails in the pipeline

### Possible causes

1. The pipeline uses a different environment.
2. A required environment variable is missing.
3. The pipeline uses a different product or test version.
4. Test data is not available to the pipeline.
5. The pipeline service account has insufficient permissions.

### Resolution

Compare the local and pipeline configurations:

```diff
- environment: DEVELOPMENT
+ environment: TEST

- user: developer-user
+ user: pipeline-service-account
```

Confirm that the pipeline provides the required values:

```yaml
variables:
  TEST_ENVIRONMENT: TEST
  TEST_SUITE: CUSTOMER-REGRESSION
```

> [!TIP]
> Record the product version, environment, test-suite name, and configuration revision in the pipeline log. This information makes failed runs easier to reproduce.

---

## Collect diagnostic information

If the problem continues, collect the following information before contacting support:

- Product name and version
- Operating system
- Test name and test type
- Target environment
- Date and time of the failure
- Complete error message
- Steps required to reproduce the problem
- Relevant log files
- Recent configuration or application changes

Use a structure similar to the following example:

```json
{
  "product": "BMC AMI DevX Total Test",
  "testName": "Customer discount test",
  "testType": "Unit test",
  "environment": "TEST",
  "status": "Failed",
  "errorCode": "TT-104"
}
```

> [!WARNING]
> Logs and diagnostic files might contain user IDs, system names, business data, or other sensitive information. Review and sanitize the files before sharing them.

## Error-code reference

<details>
<summary><strong>TT-104: Program not found</strong></summary>

**Meaning:** The configured program could not be located in the selected environment.

**Action:** Verify the program name, environment, deployment status, and load-library configuration.

</details>

<details>
<summary><strong>TT-205: Authentication failed</strong></summary>

**Meaning:** The target system rejected the supplied credentials.

**Action:** Verify the user ID, reset expired credentials, and confirm the required permissions.

</details>

<details>
<summary><strong>TT-310: Test execution timed out</strong></summary>

**Meaning:** The test did not finish within the configured time limit.

**Action:** Review the program execution, connected services, test data, and timeout configuration.

</details>

## Footnotes

Virtualized testing can help isolate the program from dependencies that are unavailable, costly, or difficult to control.[^1] Non-virtualized testing uses available resources and dependencies in the connected environment.[^2]

[^1]: The precise virtualization capabilities depend on the product version, licensed components, and test configuration.
[^2]: A non-virtualized test can affect data or connected systems. Run it only in an approved test environment.

## Related topics

- [BMC AMI DevX Total Test overview](overview.md)
- [Getting started with BMC AMI DevX Total Test](getting-started.md)
- [Creating your first test](user-guide.md)
- [Running a regression test suite](installation.md)
- [Collecting diagnostic information](administration.md)

## Next steps

After resolving the problem:

1. Run the test again.
2. Confirm that the test produces the expected result.
3. Add the test to the appropriate regression suite.
4. Record the resolution if the problem is likely to recur.
5. Escalate the problem if it remains unresolved.