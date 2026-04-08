# Mini-Assignment 4: Orchestration with CrewAI

## Pipeline Stages

This pipeline consists of **3 sequential CrewAI crews**:

| Stage | Crew Name | Agent Role | Task Description |
|-------|-----------|------------|------------------|
| 1 | Research Crew | Senior Research Analyst | Gathers comprehensive information about a given topic |
| 2 | Analysis Crew | Strategic Analyst | Provides SWOT analysis and strategic recommendations |
| 3 | Summary Crew | Executive Summarizer | Creates a concise executive summary |

### Data Flow
Input Topic → Research Crew → Research Report → Analysis Crew → Strategic Analysis → Summary Crew → Executive Summary

**Example:**
"AI in healthcare" → [Research] → 5-section research report → [Analysis] → SWOT + recommendations → [Summary] → 1-page exec summary

## How to Run

### 1. Setup

```bash
# Install dependencies
pip install -r requirements.txt

# Create .env file with your DeepSeek API key
echo "DEEPSEEK_API_KEY=your_key_here" > .env

# Run with default topic (AI in healthcare)
python pipeline.py

# Run with custom topic
python pipeline.py --topic "Climate change solutions"

# Force a fresh run (ignore checkpoint)
python pipeline.py --reset

# Show resume capability demo
python pipeline.py --demo-resume
## Resume After Failure

The pipeline saves a **checkpoint.json** file after each successful stage. If the pipeline fails, simply run the same command again - it will skip completed stages and resume from where it failed.

## Reliability Features

| Feature | Implementation |
|---------|----------------|
| Retry Logic | 3 retries with exponential backoff (2s, 4s, 8s) |
| Checkpointing | JSON file saves after each stage |
| Resume Capability | Skips completed stages automatically |

## Challenges Encountered

- **CrewAI Output Format**: `kickoff()` returns a `CrewOutput` object, not a string - fixed with `str(result)`
- **Data Flow Across Skipped Stages**: Updated `current_input` with checkpoint result after each skip
- **Testing Resume**: Temporarily added `raise Exception()` to simulate failures

## File Structure
├── pipeline.py
├── crews.py
├── checkpoint.json
├── output.txt
├── requirements.txt
├── .env
├── prompt-log.md
└── README.md