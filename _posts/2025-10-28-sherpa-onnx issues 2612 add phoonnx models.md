---
published: true
---
add phoonnx models #2612
  https://github.com/k2-fsa/sherpa-onnx/issues/2612

### 这个 GitHub Issue #2612 是在做什么？

这个 Issue 来自 sherpa-onnx 项目（一个 ONNX 运行时的语音识别/合成工具），标题是“add phoonnx models”（添加 phoonnx 模型）。简单来说，开 issue 的人（@JarbasAl）想请求项目团队在 sherpa-onnx 中**添加对 phoonnx 模型的支持**。下面我一步步解释清楚，避免技术细节太深。

#### 背景：Phoonnx 是什么？
- Phoonnx 是作者自己开发的 TTS（Text-to-Speech，文本转语音）引擎，仓库在 [https://github.com/TigreGotico/phoonnx](https://github.com/TigreGotico/phoonnx)。
- 它还处于**早期开发阶段**，作者自己说“不是生产就绪的”（not production-ready），但已经能用了。
- Phoonnx 的模型用 espeak（一个语音合成工具，用于生成音素）训练时，和 **Piper TTS**（另一个 TTS 系统）兼容。sherpa-onnx 项目之前已经在 PR #2530 中用过作者的这些模型。

#### Issue 的目的：想做什么？
- 作者的最新训练代码让模型的 `.json` 配置文件**稍有变化**，所以他觉得是时候正式提个 issue，让 sherpa-onnx 官方支持 phoonnx 模型。
- 重点是**只支持用 espeak + IPA（国际音标）训练的模型**，其他类型的（如非 espeak 的音素化器）还需要更多测试。
- 好处：这样 sherpa-onnx 就能直接运行 phoonnx 的语音模型了，尤其适合多语言场景（例子是阿拉伯语模型）。

#### Issue 里提供了什么？
- 一个**Python 脚本**，用来处理 phoonnx 模型，让它兼容 sherpa-onnx。具体步骤：
  1. 加载模型的 `.json` 配置。
  2. 检查配置（必须是 IPA 字母表 + espeak 音素化器，否则报错）。
  3. 生成一个 `tokens.txt` 文件（列出音素 ID 映射，跳过无效的如换行符）。
  4. 在 ONNX 模型文件里添加**元数据**（metadata），比如：
     - 模型类型：vits（一种 TTS 架构）。
     - 语言：如 "Arabic"（阿拉伯语）。
     - 声音：如 "ar-SA"（阿拉伯语-沙特）。
     - 采样率、说话人数等。
     - 标记有 espeak 支持。
  - 脚本示例针对一个叫 "miro_ar-SA.onnx" 的阿拉伯语模型。

脚本代码大致长这样（简化版）：
```python
import json
import onnx

# ... (加载配置、生成 tokens.txt 的函数)

def add_meta_data(filename, meta_data):
    model = onnx.load(filename)
    for key, value in meta_data.items():
        meta = model.metadata_props.add()
        meta.key = key
        meta.value = str(value)
    onnx.save(model, filename)

# 主函数：加载配置、验证、生成文件、添加元数据
```

#### 当前状态
- Issue 还是**开放的**，没有分配人、标签、分支或 PR。
- 没有新评论或更新（从文档和最新检查看）。

#### 为什么提这个 Issue？
本质上是作者想让 sherpa-onnx “拥抱”他的 phoonnx 项目，促进 TTS 生态整合。如果你对 TTS 感兴趣，可以去 phoonnx 仓库试试模型，或者在 Issue 下评论支持。假如你想深入，可以看原链接：[https://github.com/k2-fsa/sherpa-onnx/issues/2612](https://github.com/k2-fsa/sherpa-onnx/issues/2612)。

如果还有具体不懂的地方（如代码细节），再问我！