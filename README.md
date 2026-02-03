# Prolific AI Task Builder: Getting Started

[![Python 3.11+](https://img.shields.io/badge/python-3.11+-blue.svg)](https://www.python.org/downloads/)
[![Status: Beta](https://img.shields.io/badge/Status-Beta-orange.svg)]()
[![Purpose: Educational](https://img.shields.io/badge/Purpose-Educational-green.svg)]()

A streamlined workflow for collecting human preference data using the **Prolific AI Task Builder**. This repository demonstrates how to collect pairwise preference data at scale for **preference tuning** and AI alignment research.

This repository demonstrates how to:
1. Upload your response pairs to Prolific AI Task Builder
2. Collect human preferences at scale using Prolific's participant pool
3. Export preference data in a format ready for preference tuning and reward model training

## Features

- **Prolific Integration**: Seamless integration with Prolific's AI Task Builder for pairwise preference collection
- **Preference Tuning Output**: Export data in `(prompt, chosen, rejected)` format compatible with popular alignment frameworks like Hugging Face TRL
- **Demographic Data**: Collect participant demographics for analysis and bias detection
- **Configurable Workflows**: YAML-based configuration for study settings and task instructions
- **Python Client**: Easy-to-use Python client for interacting with Prolific's API

## Repository Structure

```
prolific-ai-task-builder-getting-started/
├── src/
│   └── prolific_ai_taskers/
│       ├── __init__.py                # Package initialization
│       ├── prolific_client.py         # Prolific API client
│       └── data_processing.py         # Data processing utilities
├── notebooks/
│   ├── prolific-ai-task-builder-getting-started.ipynb  # Main workflow notebook
│   ├── input_examples/
│   │   └── response_pairs.csv         # Example input format
│   └── output_examples/
│       ├── preferences.jsonl          # Final preference data
│       ├── demographic.csv            # Participant demographics
│       ├── votes_preferences.csv      # Aggregated vote counts
│       └── raw_preferences.csv        # Raw preference data
├── config.yaml                        # Prolific study configuration
├── environment.yaml                   # Conda environment file
└── .env.example                       # Environment variables template
```

## Getting Started

### Prerequisites

- Python 3.11+
- Prolific account with API token ([sign up here](https://app.prolific.com/register))
- Pre-generated response pairs in CSV format (see [input_examples](notebooks/input_examples/) for format)

### Installation

We recommend using a virtual environment.

**Using Conda:**

```bash
conda env create -f environment.yaml
conda activate prolific-ai-task-builder
```

### Configuration

1. **Set up your Prolific Account**: Create a Prolific account and obtain your API credentials (API token, workspace ID, and project ID).

2. **Copy the environment file template**:
   ```bash
   cp .env.example .env
   ```

3. **Edit your [.env](.env) file** with your Prolific API credentials:
   ```bash
   # Prolific API Configuration
   PROLIFIC_API_TOKEN=your_prolific_api_token_here
   PROLIFIC_WORKSPACE_ID=your_workspace_id_here
   PROLIFIC_PROJECT_ID=your_project_id_here
   ```

4. **Customize [config.yaml](config.yaml)** to configure:
   - Study settings (reward, completion time, participants per task)
   - Task instructions and question phrasing
   - Participant eligibility criteria
   - Device compatibility

5. **Prepare your response pairs**: Create a CSV file with your AI-generated response pairs. See [notebooks/input_examples/response_pairs.csv](notebooks/input_examples/response_pairs.csv) for the required format:
   - Required columns: `prompt`, `response_a`, `response_b`
   - Each row represents one pairwise comparison task

## Usage

Use the Jupyter notebook to run the workflow:

**Notebook**: [notebooks/prolific-ai-task-builder-getting-started.ipynb](notebooks/prolific-ai-task-builder-getting-started.ipynb)

The notebook walks through the complete workflow step-by-step, with explanations and visualizations.

### Workflow Steps

#### Step 1: Prepare Your Data
- Prepare your response pairs CSV file with columns: `prompt`, `response_a`, `response_b`
- Review [notebooks/input_examples/response_pairs.csv](notebooks/input_examples/response_pairs.csv) for the expected format
- Ensure your prompts and responses are appropriate for human evaluation

#### Step 2: Upload to Prolific
- Load your response pairs into the notebook
- Use the `ProlificClient` to create an AI Task Builder batch
- Upload your response pairs to Prolific's platform
- The client handles formatting and batch creation automatically

#### Step 3: Create and Publish Study
- Configure your study settings in [config.yaml](config.yaml)
- Create a Prolific study linked to your AI Task Builder batch
- Set participant requirements, rewards, and completion time
- Publish the study to start collecting preferences

#### Step 4: Monitor and Download Results
- Monitor task completion through the Prolific dashboard
- Once complete, download the preference data and demographics
- Process the results using the `data_processing` utilities

**Key outputs:**
- `preferences.jsonl` - Final preference data in `(prompt, chosen_response, rejected_response)` format
- `votes_preferences.csv` - Aggregated vote counts per response pair
- `demographic.csv` - Participant demographic information
- `raw_preferences.csv` - Raw preference data from Prolific

### Example Configuration

The [config.yaml](config.yaml) file allows you to customize your Prolific study:

```yaml
prolific:
  # Task configuration
  task_schema:
    task_name: "Compare two AI-generated responses"
    task_introduction: "Instructions shown to participants"
    task_question: "Which response is better?"
    tasks_per_group: 10  # Comparisons per participant

  # Study settings
  study_setup:
    estimated_completion_time: 5  # minutes
    reward: 200  # cents (payout per participant)
    participants_per_task: 5  # annotators per comparison
    device_compatibility: ["desktop"]

    # Participant filters
    filters:
      age:
        lower: 18
        upper: 70
```

## Output Format

The final output (`preferences.jsonl`) is formatted for direct use in preference tuning and reward model training:

```json
{
  "prompt": "How do I make homemade pasta from scratch?",
  "chosen_response": "Making your own pasta dough is easier than you think...",
  "rejected_response": "How to make homemade pasta from scratch 1. Mix..."
}
```

This format is compatible with popular preference tuning frameworks like [Hugging Face TRL](https://github.com/huggingface/trl).

See [notebooks/output_examples/](notebooks/output_examples/) for example outputs from a completed study.


## Use Cases

- **Preference Tuning**: Collect human preference data for aligning AI models with human values
- **AI Alignment Research**: Gather preferences for training safer and more helpful AI systems
- **Model Comparison**: Evaluate and compare responses from different models or configurations
- **Response Quality Assessment**: Collect feedback on helpfulness, accuracy, safety, and other quality dimensions

## Contributing

Contributions are welcome! Please feel free to submit issues or pull requests.

## Important Notice

> This project is provided **as-is** for **educational and research purposes only**.
> - 🔬 **Beta Status**: This is experimental code and may contain bugs or incomplete features
> - 📚 **Not Maintained**: No active development or support is provided
> - 🎓 **Educational Use**: Intended as a learning resource for preference data collection workflows
> - ⚖️ **Use at Your Own Risk**: Test thoroughly before using in production environments
