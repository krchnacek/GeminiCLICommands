# Gemini CLI Commands

This repository contains a set of commands for the Gemini CLI that help streamline the software development workflow. These commands are designed to be used in a specific order to ensure a consistent and efficient process from planning to implementation and testing.

## About Gemini CLI Commands

A Gemini CLI command is a TOML file that defines a specific task for the AI agent. It encapsulates the prompt, role, objective, and constraints for a particular operation, allowing for reproducible and structured AI interactions.

## Installation

To install these commands, simply clone this repository into your Gemini CLI commands directory (typically `~/.gemini/commands`).

```bash
git clone <repository_url> ~/.gemini/commands
```

## Usage

Once installed, you can run any of these commands using the `gemini` CLI tool followed by the command name (the filename without the `.toml` extension).

```bash
gemini <command_name> [arguments]
```

## Workflow

The intended workflow is as follows:

1.  **`clarify_spec [feature_id]`**: Reviews and clarifies the functional specification, updating it and creating/modifying test scenarios.
2.  **`clarify_design [feature_id]`**: Reviews and clarifies the technical design, updating the design document.
3.  **`tasks [feature_id]`**: Create or update the implementation plan, including testing and documentation strategies.
4.  **`implement [feature_id]`**: Implement the tasks defined in the specification file, using a TDD/BDD approach.
5.  **`tests`**: Run unit and integration tests to verify the implementation.
6.  **`e2e_tests`**: Run the End-to-End tests to verify the feature in a production-like environment.

## Commands

### `clarify_spec`

*   **Purpose:** Initiates a combined review and clarification process for a functional specification. It reviews the specification for clarity, completeness, consistency, and testability, presents the feedback to the user, and then interactively clarifies and updates the functional specification, including creating or modifying Gherkin test scenarios.
*   **Why to use it:** To ensure functional requirements are robust, unambiguous, and fully tested before implementation. This command prevents breaking changes to existing features by analyzing cross-feature impacts.
*   **How to use it:** `gemini clarify_spec <feature_id>`
*   **What it does:**
    1.  Finds the functional specification file based on the `feature_id`.
    2.  Analyzes the `## Functional Specification` section for clarity, completeness, consistency, and testability.
    3.  Presents the review feedback directly to the user.
    4.  Interactively clarifies the feedback with the user, updating the specification based on their input.
    5.  Creates or modifies `.feature` files with Gherkin scenarios for E2E testing.
    6.  **Performs cross-feature impact analysis** by scanning all existing `.feature` files in the project.
    7.  **Requests developer approval** if conflicts with existing features are detected (APPROVED/MODIFY/REJECT).
    8.  Documents any approved changes in a `## Change Impact` section.
    9.  Updates the original specification file.

### `clarify_design`

*   **Purpose:** Initiates a combined review and clarification process for a technical design. It reviews the design for soundness, scalability, maintainability, and security, presents the feedback to the user, and then interactively clarifies and updates the technical design document.
*   **Why to use it:** To ensure the technical architecture is sound, scalable, maintainable, and secure before implementation. This command prevents architectural conflicts by analyzing impacts on existing features.
*   **How to use it:** `gemini clarify_design <feature_id>`
*   **What it does:**
    1.  Finds the technical design document based on the `feature_id`.
    2.  Analyzes the `## Technical Design` section for soundness, scalability, maintainability, and security.
    3.  Presents the review feedback directly to the user.
    4.  Interactively clarifies the feedback with the user, updating the design document based on their input.
    5.  **Performs cross-feature impact analysis** by scanning existing `.feature` files and technical design documents.
    6.  **Requests developer approval** if architectural conflicts are detected (APPROVED/MODIFY/REJECT).
    7.  Documents any approved changes in a `## Change Impact` section.
    8.  Updates the original design document.

### `tasks`

*   **Purpose:** Creates or updates an implementation plan for a feature.
*   **Why to use it:** To break down a feature into actionable tasks, define a testing strategy, and plan for documentation. This command also detects potential conflicts with existing features early in the planning process.
*   **How to use it:** `gemini tasks <feature_id>`
*   **What it does:**
    1.  Finds the feature specification file or proposes a new one.
    2.  Investigates the codebase to understand the required changes.
    3.  Defines a testing strategy (TDD style, integration points, E2E coverage via `.feature` files).
    4.  Creates a documentation plan (build/run instructions, deployment, E2E testing).
    5.  Drafts a list of implementation tasks with mandatory final tasks (run tests + build).
    6.  **Performs cross-feature impact analysis** to identify potential conflicts.
    7.  Presents the plan with any detected conflicts to the user.
    8.  Asks for user confirmation before saving the plan.

### `implement`

*   **Purpose:** Implements the tasks defined in the specification file using a strict TDD/BDD approach.
*   **Why to use it:** To ensure that the implementation follows the plan and adheres to TDD principles. This command automates the Red-Green-Refactor cycle for each task.
*   **How to use it:** `gemini implement <feature_id>`
*   **What it does:**
    1.  Finds the specification file based on `feature_id`.
    2.  Loads context by reading `## Functional Specification` and `## Technical Design` sections.
    3.  Identifies associated `.feature` files for acceptance criteria (not executed during TDD loop).
    4.  Iterates through unchecked tasks in `## Implementation Tasks`:
        *   Writes failing unit/integration tests (Red)
        *   Writes minimal implementation code to pass tests (Green)
        *   Refactors for clarity and maintainability
        *   Verifies all tests pass
        *   Marks task as complete (`- [x]`)
        *   Commits changes with descriptive message
    5.  Repeats until all tasks are complete.
    6.  **Note:** Does NOT modify `## Functional Specification` or `## Technical Design` sections.

### `tests`

*   **Purpose:** Runs all unit and integration tests in the application.
*   **Why to use it:** To quickly verify that changes haven't broken existing functionality during the development process (standard "Red-Green-Refactor" loop).
*   **How to use it:** `gemini tests`
*   **What it does:**
    1.  Checks the environment for dependencies.
    2.  Executes the project's configured unit and integration test runner (e.g., `npm test`, `mvn test`).
    3.  Reports success or failure with details.

### `e2e_tests`

*   **Purpose:** Runs the automated End-to-End (E2E) tests.
*   **Why to use it:** To verify that the application works as expected from the user's perspective in a production-like environment. This is the final step before deploying a feature.
*   **How to use it:** `gemini e2e_tests`
*   **What it does:**
    1.  Checks that the application is running in the correct test environment.
    2.  Executes the E2E test suite.
    3.  Reports the test results.

### `build`

*   **Purpose:** Builds the application from source code.
*   **Why to use it:** To compile/build the application before running or deploying it.
*   **How to use it:** `gemini build`
*   **What it does:**
    1.  Checks documentation for build instructions.
    2.  Finds and executes the project's build command (e.g., `npm run build`).
    3.  Verifies build artifacts and reports success or failure.

### `run`

*   **Purpose:** Starts the application locally.
*   **Why to use it:** To run the application in a local development environment for manual testing or development.
*   **How to use it:** `gemini run`
*   **What it does:**
    1.  Checks for dependencies.
    2.  Finds and executes the project's start command (e.g., `npm start`).
    3.  Monitors output for errors or success messages.
    4.  Note: Assumes the app is already built (run `build` first if needed).