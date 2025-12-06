# CSE 476 Final Project

An agent that solves complex problems across multiple domains using three inference time reasoning techniques: Baseline, Chain of Thought (CoT), and Self Consistency (SC).

## Overview

This project implements an inference time reasoning agent. It dynamically selects the most appropriate solving technique based on problem characteristics to optimize both accuracy and efficiency.

## Techniques Implemented

### 1. Baseline Solver
- **Use case**: Multiple-choice, knowledge retrieval, common-sense questions
- **API calls**: 1 per question
- **Description**: Simple direct prompting for factual questions where step-by-step reasoning is unnecessary

### 2. Chain of Thought (CoT) Solver
- **Use case**: Math calculations, coding problems, general reasoning
- **API calls**: 1 per question  
- **Description**: Prompts the model to "think step by step" with sophisticated answer extraction using priority-based pattern matching

### 3. Self-Consistency (SC) Solver
- **Use case**: Complex planning and scheduling problems
- **API calls**: 3 per question (configurable)
- **Description**: Generates multiple reasoning paths and uses majority voting to select the most consistent answer

## Project Structure

```
├── FinalCode.ipynb                          # Main notebook with all code
├── cse_476_final_project_test_data.json     # Input test data
├── cse_476_final_project_answers.json       # Output answers
├── data/
│   └── cse476_final_project_dev_data.json   # Development data for testing
└── README.md
```

## Key Classes

| Class | Cell | Description |
|-------|------|-------------|
| `LLMClient` | 2 | API wrapper with retry logic and call tracking |
| `DataLoader` | 4 | Loads and manages question data |
| `Problem` | 5 | Question wrapper class |
| `BaselineSolver` | 7 | Direct prompting solver |
| `ChainOfThoughtSolver` | 8 | Step by step reasoning solver |
| `SelfConsistencySolver` | 9 | Majority voting solver |
| `IntelligentAgent` | 12 | Orchestrates technique selection |

## Key Methods

- `_detect_domain()` : Content based domain classification using keyword matching
- `_extract_answer()` : 8 priority answer extraction pipeline
- `_extract_code_block()` : Extracts code from model responses
- `post_process_answer()` : Cleans LaTeX artifacts and normalizes output
- `build_answers()` : Main processing loop with progress tracking and checkpointing

## Installation & Setup

### Requirements
- Python 3.8+
- Jupyter Notebook

### Dependencies
```bash
pip install requests tqdm
pip install requests
```

## How to Run

1. Clone this repository:
```bash
git clone https://github.com/YOUR_USERNAME/YOUR_REPO.git
cd YOUR_REPO
```

2. Place the test data file `cse_476_final_project_test_data.json` in the working directory

3. Open and run `FinalCode.ipynb`:
```bash
jupyter notebook FinalCode.ipynb
```

4. Run all cells sequentially. The notebook will:
   - Initialize the solvers
   - Process all 6,208 questions
   - Save progress every 50 questions to `temp_answers.json`
   - Output final answers to `cse_476_final_project_answers.json`

## Features

- **Auto-resume**: If interrupted, the notebook resumes from the last checkpoint
- **Progress tracking**: tqdm progress bar shows current progress and API call statistics
- **Validation**: Automatic validation of output format before saving
- **Error handling**: Robust error handling with retry logic for API failures

## API Configuration

The agent uses the course-provided API:
- **Endpoint**: `http://10.4.58.53:41701/v1`
- **Model**: `bens_model`
- **Max API calls per question**: 20 (agent uses 1-3)

## Output Format

The output file contains a JSON array with one object per question:
```json
[
  {"output": "answer1"},
  {"output": "answer2"},
  ...
]
```
