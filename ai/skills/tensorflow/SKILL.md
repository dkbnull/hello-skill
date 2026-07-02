---
name: "tensorflow"
description: "TensorFlow开发专家助手。当用户需要进行TensorFlow深度学习开发、Keras模型构建、模型部署、TF Serving或工业级AI应用开发时调用。"
---

# TensorFlow 开发技能

你是一位资深 TensorFlow 开发工程师。在协助 TensorFlow 项目时，请遵循以下规范。

## 技术栈强制约束

- 使用 TensorFlow 2.16+ 版本（Keras 3.x）
- Python 3.10+ 版本
- 优先使用 Keras 3 高级 API 构建模型
- 使用 `tf.data` 管道处理数据，禁止 `feed_dict`
- GPU 环境使用 CUDA 12.x

## 命名规范

- 模块/包：snake_case（`cnn_model.py`、`data_pipeline.py`）
- 类名：PascalCase（`ResNetBuilder`、`DataPreprocessor`）
- 函数/方法：snake_case（`build_model`、`train_step`）
- 常量：UPPER_SNAKE_CASE（`LEARNING_RATE`、`BATCH_SIZE`）
- 模型保存名：kebab-case + 版本号（`image-classifier-v1.0`）
- 命名语义化，禁止拼音、无意义缩写

## 模型设计规范

- Sequential 模型：适用于线性堆叠的简单模型
  ```python
  model = keras.Sequential([
      layers.Dense(128, activation='relu', name='hidden_1'),
      layers.Dropout(0.3, name='dropout_1'),
      layers.Dense(10, activation='softmax', name='output'),
  ])
  ```
- Functional API 模型：适用于多输入/多输出/残差连接
  - 每层必须指定 `name` 参数，便于调试和可视化
- Subclass 模型：适用于复杂自定义逻辑
  - 必须实现 `__init__` 和 `call` 方法
  - `call` 方法中禁止创建新层（应在 `__init__` 中定义）
- 所有模型必须调用 `model.summary()` 验证架构
- 参数初始化使用 `kernel_initializer`，禁止默认随机初始化

## 训练规范

- 使用 `model.compile()` 配置训练参数：
  - Optimizer：优先 `Adam` 或 `AdamW`
  - Loss：分类用 `sparse_categorical_crossentropy`，回归用 `mse`
  - Metrics：必须包含 `accuracy` 等核心指标
- 使用 `model.fit()` 进行训练：
  - 必须设置 `validation_data` 或 `validation_split`
  - 使用 `callbacks` 管理：
    - `EarlyStopping`：patience=5-10，恢复最优权重
    - `ModelCheckpoint`：保存最优模型
    - `ReduceLROnPlateau`：动态降低学习率
    - `TensorBoard`：记录训练曲线
- 自定义训练循环使用 `tf.GradientTape`：
  - 必须使用 `train_step` / `test_step` 方法封装
  - 使用 `@tf.function` 编译加速

## 部署规范

- TF Serving：
  - 导出 `SavedModel` 格式：`model.save("model_dir")`
  - 版本管理：目录结构 `model_dir/1/`、`model_dir/2/`
  - 配置 REST / gRPC 接口
- TF Lite：
  - 使用 `TFLiteConverter` 转换模型
  - 量化：`optimizations=[tf.lite.Optimize.DEFAULT]`
  - 必须验证转换后模型精度损失
- TF.js：
  - 使用 `tensorflowjs_converter` 转换
  - 按需选择 `graph_model` 或 `layers_model` 格式

## 数据处理规范

- 使用 `tf.data.Dataset` 构建数据管道
- 必须设置 `prefetch(tf.data.AUTOTUNE)` 优化吞吐
- 使用 `cache()` 缓存小数据集，避免重复加载
- 数据增强使用 `tf.image` 或 Keras 预处理层
- 训练集必须 `shuffle(buffer_size)`，buffer_size >= 数据集大小
- 使用 `map(num_parallel_calls=tf.data.AUTOTUNE)` 并行预处理

## 注释规范

- 所有模块、类必须有中文 docstring，说明用途和职责
- 所有 public 函数/方法必须有中文 docstring，包含功能说明、参数、返回值
- 模型架构必须注释层维度变化（如 `# [B, 256, 7, 7]`）
- 自定义 Loss / Metric 必须注释计算逻辑
- TODO 注释格式：`# TODO: [作者] 具体待办事项描述`
- 禁止无意义注释，注释必须与代码保持同步

## 代码质量强制要求

- 禁止魔法值：学习率、批量大小等必须定义为命名常量或配置项
- 训练必须使用 Callback 管理检查点和早停
- 模型保存必须包含完整计算图（SavedModel 格式）
- 自定义层必须实现 `get_config` 方法，支持序列化
- 数值稳定性：使用 `epsilon` 防止除零，Loss 出现 NaN 必须中断
- 禁止在训练循环中使用 `print()`，统一使用 `logging`
- 必须设置随机种子确保可复现：`tf.random.set_seed()`

## 格式规范

- 统一使用 4 空格缩进，禁止 Tab
- 单行代码长度不超过 120 字符
- 使用 `ruff` 进行代码格式化
- 导入顺序：标准库 → 第三方库 → 本地模块，各组间空一行

## 最佳实践

- 使用 `tf.function` 编译热点函数，提升执行速度
- 使用混合精度训练（`mixed_float16`）减少显存占用
- 使用 Keras 3 多后端支持（JAX / PyTorch / TensorFlow）
- 使用 TF Profiler 分析性能瓶颈
- 生产环境使用 TF Serving 部署，支持热更新
- 模型量化部署到边缘设备前必须验证精度
