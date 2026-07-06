# Java Gradle Quality Gate Skill (Java 11, 17, 21)

## Purpose
Enforce compilation, testing, and strict static analysis gates using the Gradle wrapper and SonarQube MCP before proposing any code commits or completion flags.

## Requirements & Environment
- **Build Tool**: Gradle (Always execute tasks using the local `./gradlew` wrapper).
- **Java Compatibility**: Detect the targeted Java version (LTS 11, 17, or 21) specified inside `build.gradle` (look for `sourceCompatibility`, `targetCompatibility`, or `toolchain`) before running build commands. Ensure compatibility with the active JDK.

## Strict Agentic Workflow
You must execute the following quality gates sequentially. Any failure at any step must halt execution immediately for remediation.

### Gate 1: Environment & Version Detection
- **Action**: Inspect `build.gradle` to identify if the project uses Java 11, 17, or 21.
- **Rule**: Tailor any generated code syntax (e.g., Records for 14+, Pattern Matching for 16+, Virtual Threads for 21+) to match the detected LTS version.

### Gate 2: Compile & Syntax Gate
- **Action**: Execute local compilation for both main and test sources.
- **Command**: `./gradlew compileJava compileTestJava`
- **Rule**: Zero compilation errors or raw type warnings allowed.

### Gate 3: Test Automation Gate
- **Action**: Run the entire unit and integration test suite.
- **Command**: `./gradlew test`
- **Rule**: 100% test pass rate required. If build outputs include a local JaCoCo report, code coverage must not drop below your pipeline threshold.

### Gate 4: SonarQube Analysis Gate
- **Action**: Invoke the source code analysis for the project.
- **Command**: `.\gradlew sonar` (use `./gradlew sonar` on Unix/Linux platforms).
- **Rule**: Run this command to push a code scan and analyze the active project context.

### Gate 5: SonarQube Issues Verification
- **Action**: Consult/query the SonarQube analysis results to identify unresolved issues.
- **Command**: `sonar list issues -p desktop-fiscal-invoice-email-batch-develop --statuses OPEN`
- **Rule**: Retrieve the project key (`-p` parameter) from the `sonar.projectKey` property in `build.gradle` (currently `desktop-fiscal-invoice-email-batch-develop`). Address and resolve all open issues on modified files before proceeding.

## Quality Indicators & Coverage Baselines
- **Indicator Baseline File**: Refer to the local metric definitions in [.resources/indicators.json](file:///c:/Users/mgiov/IdeaProjects/desktop-fiscal-invoice-email-batch/.agents/skills/java-quality-gate/.resources/indicators.json).
- **Enforcement Rules**:
  1. **Non-Regression Rule**: New code and tests must NOT decrease any of the current quality indicators. They must only improve or maintain them.
  2. **Baseline Target Metrics**:
     - *Instruction Coverage*: >= 83.75%
     - *Branch Coverage*: >= 79.86%
     - *Line Coverage*: >= 81.96%
     - *Method Coverage*: >= 84.84%
     - *Class Coverage*: >= 86.21%
     - *Open Issues (Sonar)*: 0

## Error Remediation Protocol
If any Gradle task fails or a SonarQube rule is breached:
1. Parse the local terminal output or build log files immediately.
2. Formulate a targeted, permanent code fix.
3. Re-run the specific failed gate. Do not bypass or move to the next gate until it returns a clean success status.
