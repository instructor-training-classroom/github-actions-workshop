# GitHub Actions Workshop - Part 1 & Part 2 Workflows

This repository demonstrates two interconnected GitHub Actions workflows that showcase artifact handling and file operations.

## Workflows Overview

### Part 1 - Build and Upload Artifact (`part1.yml`)

**Triggers:**
- Manual trigger via `workflow_dispatch`
- Automatic trigger on push to `main` branch

**What it does:**
1. Creates sample files with build information
2. Copies application files from the repository
3. Creates a zip archive of all files
4. Uploads the archive as a GitHub Actions artifact

**File Operations:**
- Creates directories (`mkdir`)
- Creates text files with build metadata
- Copies files from the repository
- Creates zip archives using `zip` command
- Lists directory contents

**Artifact:** `build-output`
- Contains: `project-bundle.zip` and `summary.md`
- Retention: 5 days

---

### Part 2 - Download and Process Artifact (`part2.yml`)

**Triggers:**
- Automatically runs when Part 1 workflow completes successfully
- Uses `workflow_run` trigger

**What it does:**
1. Downloads the artifact from Part 1
2. Unzips the archive
3. Reads and processes the files
4. Creates transformed/combined output files
5. Creates a new zip archive with processed files
6. Uploads the processed files as a new artifact

**File Operations:**
- Downloads artifacts from previous workflow run
- Unzips archives using `unzip` command
- Reads file contents with `cat`
- Copies files with new names
- Combines multiple files into one
- Creates new zip archives
- Generates markdown reports

**Artifact:** `processed-output`
- Contains: All processed files and a processing report
- Retention: 5 days

---

## How They Work Together

```
┌─────────────────────────────────────────────────────────────┐
│                        Part 1 Workflow                       │
│  (Triggered by push to main or manual dispatch)             │
│                                                              │
│  1. Create files                                            │
│  2. Copy project files                                      │
│  3. Zip files → project-bundle.zip                         │
│  4. Upload artifact: build-output                          │
└──────────────────────┬──────────────────────────────────────┘
                       │
                       │ (workflow_run trigger)
                       │
                       ▼
┌─────────────────────────────────────────────────────────────┐
│                        Part 2 Workflow                       │
│     (Automatically triggered when Part 1 completes)         │
│                                                              │
│  1. Download artifact: build-output                         │
│  2. Unzip: project-bundle.zip                              │
│  3. Process files (read, transform, combine)               │
│  4. Zip files → processed-bundle.zip                       │
│  5. Upload artifact: processed-output                      │
└─────────────────────────────────────────────────────────────┘
```

---

## Running the Workflows

### Option 1: Manual Trigger
1. Go to the "Actions" tab in GitHub
2. Select "Part 1 - Build and Upload Artifact"
3. Click "Run workflow"
4. Part 2 will automatically run after Part 1 completes

### Option 2: Automatic Trigger
1. Push a commit to the `main` branch
2. Part 1 will automatically run
3. Part 2 will automatically run after Part 1 completes

---

## Key Features Demonstrated

### Artifact Management
- ✅ Uploading artifacts with `actions/upload-artifact@v4`
- ✅ Downloading artifacts with `actions/download-artifact@v4`
- ✅ Cross-workflow artifact sharing
- ✅ Setting retention policies

### File Operations
- ✅ Creating directories and files
- ✅ Copying files
- ✅ Creating zip archives
- ✅ Extracting zip archives
- ✅ Reading and processing file contents
- ✅ Combining multiple files
- ✅ Listing and inspecting files

### Workflow Orchestration
- ✅ Chaining workflows with `workflow_run`
- ✅ Conditional execution based on previous workflow status
- ✅ Passing data between workflows via artifacts
- ✅ Multiple trigger types (push, workflow_dispatch, workflow_run)

---

## Viewing Results

After the workflows run:

1. **Check Workflow Status:**
   - Go to Actions tab → Select workflow run
   - View logs for each step

2. **Download Artifacts:**
   - In the workflow run summary, scroll to "Artifacts"
   - Download `build-output` (from Part 1)
   - Download `processed-output` (from Part 2)

3. **Inspect Logs:**
   - Each step shows detailed output
   - File contents are displayed in logs
   - Summary reports are printed

---

## File Structure Created

### Part 1 Output:
```
artifacts/
├── data/
│   ├── hello.txt
│   ├── build-info.txt
│   ├── appsettings.json
│   └── sample-test.cs
├── project-bundle.zip
└── summary.md
```

### Part 2 Output:
```
output/
├── processed-hello.txt
├── processed-build-info.txt
├── combined-report.txt
├── processed-bundle.zip
└── part2-report.md
```

---

## Advanced Topics

### Customization Ideas:
- Modify file operations to suit your needs
- Add build steps (compile, test, etc.)
- Add deployment steps in Part 2
- Integrate with other GitHub Actions
- Add notifications (Slack, email, etc.)

### Security Notes:
- Artifacts are only accessible within the repository
- Use `secrets.GITHUB_TOKEN` for authentication
- Set appropriate retention days to manage storage

---

## Troubleshooting

**Part 2 doesn't run automatically:**
- Check that Part 1 completed successfully
- Verify the workflow name in `workflow_run` matches exactly
- Ensure you have proper permissions

**Artifact download fails:**
- Check retention period (artifacts expire after 5 days)
- Verify artifact name matches
- Check workflow run ID is correct

**File operations fail:**
- Check file paths (case-sensitive on Linux runners)
- Verify files exist before operations
- Check zip/unzip syntax for your OS

---

## Resources

- [GitHub Actions Documentation](https://docs.github.com/en/actions)
- [Workflow Syntax](https://docs.github.com/en/actions/using-workflows/workflow-syntax-for-github-actions)
- [Artifacts Documentation](https://docs.github.com/en/actions/using-workflows/storing-workflow-data-as-artifacts)
- [Workflow Run Trigger](https://docs.github.com/en/actions/using-workflows/events-that-trigger-workflows#workflow_run)
