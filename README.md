# Financial Annual Report Summary Generation and Evaluation System

This project provides a complete financial annual report summary generation and evaluation system, using Large Language Models (LLM) to generate financial annual report summaries for non-professional investors, and conducting multi-dimensional quality evaluation based on the G-Eval framework.

## Project Overview

This project includes two core functions:

1. **Summary Generation** (`summary_generation.ipynb`): Uses Qwen2.5-14B-Instruct model to generate financial annual report summaries
2. **Quality Evaluation** (`evaluate_reports_geval.ipynb`): Conducts four-dimensional evaluation of generated summaries based on the G-Eval framework

## Key Features

### Summary Generation Features
- Supports four prompt evolution stages (Baseline → Role & Background → Constraints → CoT & Lay-summary)
- Automatically selects One-shot or Map-Reduce strategy for long text processing
- Batch processing of multiple annual report files
- Supports resume from breakpoint (automatically skips already generated reports)

### Evaluation System
Based on four evaluation dimensions defined in the paper:
1. **Numerical Factuality**: Checks the accuracy of numerical data and whether numerical hallucinations exist
2. **Readability & Accessibility**: Evaluates whether professional terminology has been successfully converted into easy-to-understand language
3. **Coherence**: Checks logical coherence and information fragmentation when processing long texts
4. **Informativeness**: Verifies whether core financial sections and risk factors are completely covered

## Project Structure

```
.
├── Data/                          # Data directory
│   ├── Microsoft/                # Microsoft annual reports (2017-2025)
│   └── Nvidia/                   # Nvidia annual reports (2018-2025)
├── summary_generation.ipynb      # Summary generation script
├── evaluate_reports_geval.ipynb  # Evaluation script
├── README.md                      # Project documentation
├── .gitignore                     # Git ignore file
└── .env.example                   # Environment variable example file
```

##  Quick Start

### Environment Requirements

- Python 3.11+
- GPU with CUDA support (A800 or equivalent performance GPU recommended)
- At least 2 GPUs (for tensor parallel)

### Install Dependencies

```bash
# Install basic dependencies
pip install vllm transformers pandas pymupdf tqdm

# Install evaluation-related dependencies
pip install deepeval openai
```

### Usage Steps

#### 1. Generate Summaries

Open `summary_generation.ipynb` and execute in order:

1. **Configure paths**: Modify `INPUT_DIR` and `OUTPUT_DIR` to your actual paths
2. **Configure model**: Ensure `MODEL_DIR` points to your Qwen2.5-14B-Instruct model directory
3. **Select stage**: Set `STAGES_TO_RUN` in Cell 5 (can be a single stage or a list)
4. **Run generation**: Execute all cells, the system will automatically process all PDF files

#### 2. Evaluate Summaries

Open `evaluate_reports_geval.ipynb` and execute in order:

1. **Configure paths**: Modify `OUTPUT_DIR` and `INPUT_DIR` to your actual paths
2. **Set API key**: Ensure the `OPENAI_API_KEY` environment variable is set
3. **Run evaluation**: Execute all cells, the system will evaluate summaries from all stages

## Output Description

### Summary Generation Output

Each stage will generate in the `OUTPUT_DIR/{stage_name}/` directory:
- `{filename}_short_report.txt`: Generated summary text
- `{filename}_meta.json`: Metadata (including generation mode, token count, etc.)

### Evaluation Output

Evaluation results are saved in the `OUTPUT_DIR/evaluations_geval/` directory:
- `{stage}_{filename}_evaluation.json`: Detailed evaluation results for a single report
- `evaluation_summary.csv`: Summary table of all evaluation results

## Configuration Description

### Summary Generation Parameters

The following parameters can be adjusted in `summary_generation.ipynb`:

- `chunk_tokens`: Chunk size (default 6000)
- `overlap_tokens`: Chunk overlap size (default 300)
- `tensor_parallel_size`: Number of parallel GPUs (default 2)
- `max_model_len`: Maximum model length (default 32768)

### Evaluation Parameters

The following can be adjusted in `evaluate_reports_geval.ipynb`:

- `threshold`: Evaluation pass threshold (default 0.7)
- `model`: Model used for evaluation (default gpt-3.5-turbo)

## Prompt Evolution Stage Description

1. **Stage 1 - Baseline**: Simple and direct summary prompt
2. **Stage 2 - Role & Background**: Adds role setting and target audience
3. **Stage 3 - Constraints**: Adds constraints and format specifications
4. **Stage 4 - CoT & Lay-summary**: Adds chain of thought and popularization enhancement

## Notes

1. **Model Path**: Ensure the model path is correct and model files are complete
2. **GPU Memory**: Ensure GPU has sufficient memory to load the model (Qwen2.5-14B requires approximately 28GB VRAM)
3. **File Paths**: Windows and Linux path formats differ, pay attention to using the correct path separator

## Contributing

Welcome to submit Issues and Pull Requests!

##  License

This project is for academic research use only.

## Acknowledgments

- Uses [vLLM](https://github.com/vllm-project/vllm) for model inference
- Uses [DeepEval](https://github.com/confident-ai/deepeval) for quality evaluation
- Uses [Qwen2.5](https://github.com/QwenLM/Qwen2.5) as the base model

## Contact

If you have any questions, please submit an Issue or contact the project maintainer.
