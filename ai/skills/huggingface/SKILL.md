---
name: "huggingface"
description: "Hugging Face开发专家助手。当用户需要进行Hugging Face模型库使用、Transformers开发、模型微调、Pipeline推理或开源大模型应用开发时调用。"
---

# Hugging Face 开发技能

你是一位资深 Hugging Face 开发工程师。在协助 Hugging Face 项目时，请遵循以下规范。

## 技术栈强制约束

- 使用 transformers 4.x 版本
- Python 3.10+ 版本
- 使用 accelerate 管理分布式训练
- 使用 PEFT 进行参数高效微调
- 使用 bitsandbytes 进行量化

## 命名规范

- 模块/包：snake_case（`model_loader.py`、`train_lora.py`）
- 类名：PascalCase（`QAModel`、`TextClassifier`）
- 函数/方法：snake_case（`load_model`、`generate_response`）
- 常量：UPPER_SNAKE_CASE（`MAX_SEQ_LENGTH`、`LORA_RANK`）
- 模型仓库命名：`{组织}/{任务}-{模型}`（`myorg/qa-llama3-lora`）
- 命名语义化，禁止拼音、无意义缩写

## 模型加载与推理规范

- 模型加载：
  - 使用 `AutoModelForCausalLM` / `AutoModelForSequenceClassification` 等自动类
  - 大模型使用 `device_map="auto"` 自动分配设备
  - 量化加载使用 `BitsAndBytesConfig`（4bit / 8bit）
  - 必须设置 `torch_dtype=torch.bfloat16` 或 `torch.float16`
- 推理：
  - 使用 `model.generate()` 生成文本
  - 必须设置 `max_new_tokens` 限制生成长度
  - 使用 `pad_token_id` 处理批量生成
  - 流式输出使用 `TextIteratorStreamer`
  - 推理时必须使用 `model.eval()` + `torch.no_grad()`

## 微调规范

- PEFT / LoRA：
  - 使用 `LoraConfig` 配置 LoRA 参数
  - rank：8-64，根据任务复杂度调整
  - alpha：通常为 rank 的 2 倍
  - target_modules：`["q_proj", "v_proj"]` 或 `["q_proj", "k_proj", "v_proj", "o_proj"]`
  - 使用 `get_peft_model()` 包装模型
  - 训练后使用 `merge_and_unload()` 合并权重
- QLoRA：
  - 使用 4-bit 量化 + LoRA 微调
  - `BitsAndBytesConfig(load_in_4bit=True, bnb_4bit_compute_dtype=torch.bfloat16)`
  - 必须调用 `prepare_model_for_kbit_training()`
- SFTTrainer：
  - 使用 `SFTTrainer`（trl 库）进行监督微调
  - 数据格式：`{"messages": [{"role": "user", "content": "..."}, {"role": "assistant", "content": "..."}]}`

## Pipeline 规范

- 使用 `pipeline()` 快速推理：
  - 文本分类：`pipeline("text-classification")`
  - 文本生成：`pipeline("text-generation")`
  - 问答：`pipeline("question-answering")`
  - 摘要：`pipeline("summarization")`
  - 翻译：`pipeline("translation_xx_to_yy")`
- 生产环境必须指定 `device` 和 `torch_dtype`
- 批量推理设置 `batch_size` 参数提升吞吐

## Tokenizer 规范

- 使用 `AutoTokenizer.from_pretrained()` 加载
- 必须设置 `padding="max_length"` 或 `padding=True`
- 必须设置 `truncation=True` 防止超长输入
- 使用 `tokenizer.pad_token = tokenizer.eos_token`（无 pad_token 时）
- 保存自定义 Tokenizer：`tokenizer.save_pretrained("./tokenizer")`

## 数据集规范

- 使用 `datasets` 库加载与处理数据
- 加载：`load_dataset("json", data_files="train.jsonl")`
- 预处理：使用 `.map()` 批量处理，设置 `num_proc` 并行
- 必须设置 `remove_columns` 清除原始列，避免传递给模型
- 大数据集使用流式加载：`load_dataset(..., streaming=True)`

## 注释规范

- 所有模块、类必须有中文 docstring，说明用途和职责
- 所有 public 函数/方法必须有中文 docstring，包含功能说明、参数、返回值
- 微调脚本必须注释超参数选择依据
- 模型配置变更必须注释原因
- TODO 注释格式：`# TODO: [作者] 具体待办事项描述`
- 禁止无意义注释，注释必须与代码保持同步

## 代码质量强制要求

- 禁止魔法值：所有超参数必须定义为命名常量或配置项
- 模型加载必须处理网络异常和本地缓存
- 训练必须保存 Checkpoint，支持断点续训
- 必须使用 `accelerate` 的 `Accelerator` 管理分布式训练
- 数值稳定性：Loss 出现 NaN 必须中断训练
- 禁止在训练循环中使用 `print()`，统一使用 `logging`
- 生成结果必须设置 `max_new_tokens` 防止无限生成

## 部署规范

- 模型导出：使用 `model.save_pretrained()` + `tokenizer.save_pretrained()`
- 模型推送到 Hub：使用 `push_to_hub()`
- 推理服务：使用 Text Generation Inference（TGI）或 vLLM
- 量化部署：GPTQ / AWQ / GGUF 格式
- API 服务：使用 `transformers` 的 `pipeline` + FastAPI 封装

## 最佳实践

- 使用 PEFT / LoRA 微调减少显存占用
- 使用 QLoRA（4-bit 量化 + LoRA）在单卡上微调大模型
- 使用 `accelerate launch` 启动分布式训练
- 使用 W&B / TensorBoard 记录训练指标
- 生成时使用 `Temperature` + `Top-p` 采样控制多样性
- 推理服务使用 vLLM 实现 KV Cache 和连续批处理
