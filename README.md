# PRDbench
[Huggingface](https://huggingface.co/datasets/AGI-Eval/PRDbench) | [Github](https://github.com/AGI-Eval-Official/PRDBench)  | [Arxiv](https://arxiv.org/abs/2510.24358)

PRDBench is a benchmark designed to evaluate code agents, particularly in the context of automated software development using large language models (LLMs). With the advent of tool-equipped agents, LLMs can read large amounts of files and conduct extensive analyses, offering new possibilities for evaluation. PRDBench aims to address the limitations of existing benchmarks, particularly the lack of clear evaluation criteria and comprehensive assessment capabilities for code agents.

![Overview](PRDBench.jpg)


## Features

- 🤖 **Agent-as-a-Judge**: Use the Agent-as-a-Judge paradigm for evaluation, enabling flexible human-like question-and-answer assessments.

- ⚙️ **Structured Criteria**: Provide structured criteria and tools to make evaluations more robust.

- 💻 **Real Software Projects**: Design executable and complex tasks suitable for real software projects.

- ✨ **Simplified Annotation Process**: Significantly reduce annotation complexity, allowing annotators with basic knowledge to complete tasks.

## Requirements

- Python 3.10+
- Conda environment manager



## 🚀 Quick Start

### 1. Download PRDBench and Develop by Your CodeAgent

#### Data Format
PRDBench provides a comprehensive dataset for evaluating code agents. Each task is organized under a `{task_id}/` directory containing:

- **PRD Location**: `{task_id}/src/PRD.md` - Contains the Product Requirements Document with detailed task specifications and requirements
- **Test Plan**: `{task_id}/evaluation/detailed_test_plan.json` - Contains comprehensive test cases and evaluation criteria

When running the evaluation, the EvalAgent should:
1. Read the `{task_id}/src/PRD.md` file to understand the task requirements
2. Analyze the `{task_id}/evaluation/detailed_test_plan.json` to understand the expected functionality and test cases
3. Implement the corresponding interfaces and code based on these specifications

#### CodeAgent Development
To use your CodeAgent to run develop and debug stages of PRDBench, you can find the prompt templates in `prompt.py`:
- `DEVELOPMENT_PROMPT` - For initial task understanding and planning
- `DEBUG_PROMPT` - For troubleshooting and optimization

#### Using Your Own LLM
If you want to use your own LLM instead of the default configuration, you can apply our minimal code agent framework. Visit our repository for integration details: [Minimal Code Agent Repository](https://github.com/fulingyue/Minimal-CodeAgent)

### 2. Create Conda Environment

```bash
# Create a conda environment named evalADK with Python 3.10
conda create -n evalADK python=3.10 -y

# Activate the environment
conda activate evalADK
```

### 3. Install Dependencies

```bash
# Navigate to the project directory
cd PRDBench

# Install project dependencies
pip install -r requirements.txt
```

### 4. Configure API Keys

Edit the `Evaluation/adk_example/code_eval_agent/config.py` file and replace the following configurations:

```python
# Replace with your model name, actual API keys and base URLs
"your_model_name": LiteLlmWithSleep(
        model="openai/gpt-5", # your model
        api_base='https://api.example.com/v1/openai/native', # api base url
        api_key='your-api-key-here', # api key
        max_tokens_threshold=64000,
        enable_compression=True,
        temperature=0.1
)
```

Edit the `Evaluation/adk_example/code_eval_agent/mcp_config.py` file and replace in line 744:

```python
# Replace with your VLLM model name, actual API keys and base URLs
api_key = 'https://api.example.com/v1/openai/native'
        api_base = 'your-api-key-here'
        model_name = "model_name"
```

### 5. Evaluation
```
bash Evaluation/Evaluation_infer.sh \
  MODEL_NAME \
  ROOT_PATH \
  PORT \
  PYTHON_INTERPRETER_PORT \
  FILE_OPERATIONS_PORT \
  SYSTEM_OPERATIONS_PORT\
  source_dir
```

**Script Options:**
- `MODEL_NAME`: the name of the model to be used in the evaluation.
- `ROOT_PATH`: the base directory for storing evaluation files.
- `PORT`,`PYTHON_INTERPRETER_PORT`,`FILE_OPERATIONS_PORT`,`SYSTEM_OPERATIONS_PORT`: port numbers for various services and operations. Before using these ports, ensure they are not already occupied by other services to avoid conflicts.

Finally, you can see result_{round}.json generated under the corresponding path in workspace

### 6. Main Results

Average pass rate of code agents on PRDBench (in %).
**Leaderboard Highlights**

- `DEV`: **Claude-4.5 (69.19)**, *GPT-5.2 (62.49)*, CodeX (62.09)
- `DEBUG`: **Claude Code (70.25)**, *GPT-5.2 (69.00)*, CodeX (65.02)
- `Enhance`: Kimi-K2 (+15.65), Claude Code (+13.60), Gemini CLI (+10.22)

| Agent Type | Agent | DEV ↑ | DEBUG ↑ | Enhance ↑ |
| --- | --- | ---: | ---: | ---: |
| Minimal | GPT-5.2 | 62.49 | 69.00 | +6.51 |
| Minimal | Claude-4.5 | **69.19** | 56.40 | -12.79 |
| Minimal | Gemini-3-Pro | 22.76 | 27.28 | +4.52 |
| Minimal | Qwen3-Coder | 43.84 | 48.29 | +4.45 |
| Minimal | Kimi-K2 | 20.52 | 36.17 | +15.65 |
| Minimal | DeepSeek-V3.2 | 40.11 | 24.80 | -15.31 |
| Minimal | GLM-4.7 | 38.39 | 35.33 | -3.06 |
| Minimal | Minimax-M2 | 17.60 | 24.75 | +7.15 |
| Commercial | CodeX | 62.09 | 65.02 | +2.97 |
| Commercial | Claude Code | 56.65 | **70.25** | +13.60 |
| Commercial | Gemini CLI | 11.29 | 21.51 | +10.22 |
| Commercial | Qwen Code | 39.91 | 35.95 | -3.96 |

## Development Guide

### Project Structure

```
PRDBench/
├── EvalAgent/                        # EvalAgent code directory
│   ├── agent.py                      # Main agent program
│   ├── config.py                     # Configuration file
│   ├── mcp_servers.py                # MCP server implementation
│   ├── mcp_tools.py                  # MCP tool functions
│   └── start.sh                      # Start agent script
├── Evaluation/
│   ├── Evaluation_infer.sh           # Start evaluating script
│   └── generate_code.py              # Main interaction code
├── PRDBench/                         # Source Data of PRDBench
├── requirements.txt                  # Dependencies list
└── README.md                         # Project documentation
```

## Citation
Our paper is available now in [arxiv](https://arxiv.org/abs/2510.24358). If you use PRDBench in your research, please cite our paper:

```bibtex
@inproceedings{Fu2026, series={AAMAS},
   title={Automatically Benchmarking LLM Code Agents through Agent-driven Annotation and Evaluation},
   url={http://dx.doi.org/10.65109/HJFB4234},
   DOI={10.65109/hjfb4234},
   booktitle={Proceedings of the 25th International Conference on Autonomous Agents and Multiagent Systems},
   publisher={International Foundation for Autonomous Agents and Multiagent Systems},
   author={Fu, Lingyue and Zhang, Bolun and Guan, Hao and Zhu, Yaoming and Qiu, Lin and Liu, Weiwen and Cao, Xuezhi and Cai, Xunliang and Zhang, Weinan and Yu, Yong},
   collection={AAMAS} }
}
```

## License

This project is licensed under the MIT License.

## Contributing

Issues and Pull Requests are welcome!

## Contact

If you have any questions, please contact us through:

- Submit a GitHub Issue
- Send an email to the project maintainers

## Acknowledgments

This project is built on the [Google Agent Development Kit (ADK)](https://google.github.io/adk-docs/), which provides the foundational framework for agent development. We thank the Google ADK team for creating this excellent framework that makes agent development more accessible and modular.

---

**Note**: Please ensure that API keys and security settings are properly configured in production environments to avoid exposing sensitive information.
