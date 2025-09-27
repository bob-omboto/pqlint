# Example of using Power Query Lint API in GitHub Actions

This project provides an automated GitHub Actions workflow for linting TMDL (Tabular Model Definition Language) files using the PQLint API. The workflow automatically triggers when .tmdl files are modified and performs comprehensive code quality analysis on table definitions and expressions.

## Features

- **Automated Triggering**: Runs automatically when .tmdl files are changed
- **Comprehensive Coverage**: Analyzes all table definition files and expressions.tmdl
- **Detailed Reporting**: Provides color-coded output with severity levels (Error, Warning, Info)
- **Artifact Publishing**: Exports results as JSON artifacts for further analysis
- **Workflow Integration**: Fails the build when errors are detected to maintain code quality

# Getting Started

Follow these steps to set up the PQLint workflow in your GitHub repository.

## Prerequisites

1. **GitHub Repository**: You need access to a GitHub repository with Actions enabled
2. **PQLint API Subscription**: A valid subscription key for the PQLint API
3. **Repository Access**: The ability to fork or clone this repository into your GitHub account
4. **PBIP Project Format**: Your Power BI project must be in PBIP (Power BI Project) format with TMDL (Tabular Model Definition Language) files. This requires:
   - **Power BI Desktop** with PBIP support enabled
   - **Project saved as .pbip format** (not .pbix)
   - **TMDL semantic model structure** with separate .tmdl files for tables and expressions
   - **File structure** with any `*.SemanticModel` folder(s):
     ```
     YourProject.pbip
     YourProject.SemanticModel/
       definition/
         tables/
           Table1.tmdl
           Table2.tmdl
           ...
         expressions.tmdl
         database.tmdl
         model.tmdl
         relationships.tmdl
     
     # The workflow supports multiple semantic models:
     AnotherModel.SemanticModel/
       definition/
         tables/
           Table3.tmdl
           ...
         expressions.tmdl
     ```

## Installation Process

### Step 1: Fork or Import the Repository

1. **Navigate to this GitHub repository**
2. **Click "Fork"** to create a copy in your GitHub account
3. **Or clone/download** the repository to your local machine and push to your own GitHub repository

### Step 2: Set up Repository Secrets

The workflow uses GitHub repository secrets to store sensitive configuration like API keys.

1. **Navigate to your repository on GitHub**
2. **Go to Settings → Secrets and variables → Actions**
3. **Click "New repository secret"**
4. **Add the following secret**:
   
   | Secret Name | Value |
   |-------------|-------|
   | `PQLINT_SUBSCRIPTION_KEY` | Your PQLint API subscription key |
   
5. **Click "Add secret"** to save

### Step 3: Verify the Workflow

The workflow file is already included in this repository at `.github/workflows/pqlint.yml`, so you don't need to create it manually.

1. **Go to the "Actions" tab** in your GitHub repository
2. **You should see the "PQLint TMDL Analysis" workflow** listed
3. **Make a test change** to a .tmdl file to trigger the workflow
4. **Monitor the workflow execution** in the Actions tab

## Workflow Configuration

The workflow automatically discovers and processes all `*.SemanticModel` folders in your repository. No manual configuration of paths is required.

### How It Works

The workflow will:

1. **Automatically discover** all folders matching the pattern `*.SemanticModel` in your repository
2. **For each semantic model found**, it will scan:
   - `{SemanticModel}/definition/tables/` for all `.tmdl` table definition files
   - `{SemanticModel}/definition/expressions.tmdl` for the expressions file
3. **Process all discovered files** with PQLint analysis
4. **Report results** grouped by semantic model for easy identification

### Supported Repository Structures

The workflow supports any of these structures:

**Single Semantic Model:**
```
MyProject.pbip
MyProject.SemanticModel/
  definition/
    tables/
      *.tmdl files
    expressions.tmdl
```

**Multiple Semantic Models:**
```
Model1.SemanticModel/
  definition/
    tables/
      *.tmdl files
    expressions.tmdl

Model2.SemanticModel/
  definition/
    tables/
      *.tmdl files
    expressions.tmdl
```

**Nested Semantic Models:**
```
subfolder/
  MyModel.SemanticModel/
    definition/
      tables/
        *.tmdl files
      expressions.tmdl
```

## Supported File Types

The workflow automatically analyzes the following TMDL files from all discovered `*.SemanticModel` folders:

- **Table Definitions**: All `.tmdl` files in `{SemanticModel}/definition/tables/` directories
- **Expressions**: The `expressions.tmdl` file in `{SemanticModel}/definition/` directories

## Workflow Triggers

The workflow automatically triggers on:

- **Push to main branch** when .tmdl files are modified anywhere in the repository
- **Pull requests to main or develop branches** when .tmdl files are modified anywhere in the repository

## Understanding Results

### Severity Levels

- **Error (3)**: Critical issues that will fail the pipeline
- **Warning (2)**: Best practice violations that should be addressed
- **Info (1)**: Informational suggestions for improvement

### Output Format

For each issue found, the workflow displays:
- **Rule Name and Severity**
- **Rule ID and Category**
- **Description of the issue**
- **Location information** (line numbers, positions)
- **File name** where the issue was found
- **Semantic model** that contains the file (when multiple semantic models exist)

### Artifacts

When issues are found, the workflow publishes a JSON artifact containing:
- **Detailed results** for each file analyzed
- **Complete error information** for programmatic processing
- **Summary statistics** by severity level
- **Semantic model information** for each result

# Build and Test

## Running the Workflow

The workflow runs automatically when triggered, but you can also:

1. **Manual Run**: Go to Actions → Your Workflow → "Run workflow"
2. **Test Changes**: Create a pull request with .tmdl file modifications
3. **View Results**: Check the workflow logs for detailed analysis results

## Troubleshooting

### Common Issues

1. **"No *.SemanticModel folders found"**: Ensure your Power BI project is saved in PBIP format and contains folders ending with `.SemanticModel`
2. **"Subscription key invalid"**: Verify your PQLint API key is correct and not expired in your repository secrets
3. **"Tables directory not found"**: Check that your semantic model has the expected structure with `definition/tables/` folders
4. **"No .tmdl files found"**: Ensure your Power BI project is using TMDL format (requires newer versions of Power BI Desktop)
