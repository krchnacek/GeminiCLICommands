# Gemini CLI Commands

This repository contains a set of commands for the Gemini CLI that help streamline the software development workflow. These commands are designed to be used in a specific order to ensure a consistent and efficient process from planning to implementation and testing.

## Workflow

The intended workflow is as follows:

1.  **`review_spec [feature_id]`**: Review the functional specification for a feature.
2.  **`review_design [feature_id]`**: Review the technical design for a feature.
3.  **`tasks [feature_id]`**: Create or update the implementation plan, including testing and documentation strategies.
4.  **`implement [feature_id]`**: Implement the tasks defined in the specification file, using a TDD/BDD approach.
5.  **`e2e_tests`**: Run the End-to-End tests to verify the feature in a production-like environment.

## Commands

### `review_spec`

*   **Purpose:** Reviews a functional specification for clarity, completeness, and consistency.
*   **Why to use it:** To ensure that the requirements are well-defined and testable before starting any implementation work. This helps to prevent misunderstandings and reduces the risk of rework.
*   **How to use it:** `gemini review_spec <feature_id>`
*   **What it does:**
    1.  Finds the specification file based on the `feature_id`.
    2.  Analyzes the `## Functional Specification` section.
    3.  Creates a review file with feedback on clarity, completeness, consistency, and testability.

### `review_design`

*   **Purpose:** Reviews a technical design for soundness, scalability, and maintainability.
*   **Why to use it:** To ensure the technical design is solid before implementation begins. This helps prevent costly architectural mistakes.
*   **How to use it:** `gemini review_design <feature_id>`
*   **What it does:**
    1.  Finds the technical design document based on the `feature_id`.
    2.  Analyzes the `## Technical Design` section for soundness, scalability, maintainability, and security.
    3.  Creates a new file named `[Feature Number]_design_review.md` with detailed feedback.

### `tasks`

*   **Purpose:** Creates or updates an implementation plan for a feature.
*   **Why to use it:** To break down a feature into actionable tasks, define a testing strategy, and plan for documentation. This provides a clear roadmap for implementation.
*   **How to use it:** `gemini tasks <feature_id>`
*   **What it does:**
    1.  Finds the feature specification file or proposes a new one.
    2.  Investigates the codebase to understand the required changes.
    3.  Defines a testing strategy, including E2E tests, integration tests, and mocking.
    4.  Creates a documentation plan.
    5.  Drafts a list of implementation tasks.
    6.  Asks for user confirmation before saving the plan.

### `implement`

*   **Purpose:** Implements the tasks defined in the specification file.
*   **Why to use it:** To ensure that the implementation follows the plan and adheres to TDD/BDD principles. This command automates the cycle of writing tests, writing code, and refactoring.
*   **How to use it:** `gemini implement <feature_id>`
*   **What it does:**
    1.  Finds the specification file.
    2.  Iterates through the unchecked tasks.
    3.  Writes failing tests in a BDD style (Given, When, Then).
    4.  Writes the implementation code to make the tests pass.
    5.  Adds comments to the code to explain the "why".
    6.  Refactors the code.
    7.  Marks the task as complete in the specification file.
    8.  Commits the changes for each task.

### `e2e_tests`

*   **Purpose:** Runs the automated End-to-End (E2E) tests.
*   **Why to use it:** To verify that the application works as expected from the user's perspective in a production-like environment. This is the final step before deploying a feature.
*   **How to use it:** `gemini e2e_tests`
*   **What it does:**
    1.  Checks that the application is running in the correct test environment.
    2.  Executes the E2E test suite.
    3.  Reports the test results.