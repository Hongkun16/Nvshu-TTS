# Nvshu-TTS Dataset

[English](README.md) | 中文

女书/江永方言文本转语音数据集

## 📁 文件说明

### 核心数据文件

#### `metadata_chinese.csv` (73 KB)
- **行数**: 1,445条记录
- **格式**: `ID|汉字文本|汉字文本`
- **用途**: 汉字转录文本，与音频文件一一对应
- **示例**:
  ```
  p19-2|奉到贵家拜龙门|奉到贵家拜龙门
  p19-4|一日之荣落贵家|一日之荣落贵家
  ```

#### `metadata_ipa.csv` (134 KB)
- **行数**: 1,445条记录
- **格式**: `ID|IPA音标|IPA音标`
- **用途**: 江永方言的IPA国际音标标注
- **示例**:
  ```
  p19-2|fang13 lau21 tciang33 kue44 poe21 liang42 mai42|fang13 lau21 tciang33 kue44 poe21 liang42 mai42
  ```

#### `metadata_nvshu.csv` (93 KB)
- **行数**: 1,445条记录
- **格式**: `ID|女书标注|女书标注`
- **用途**: 女书字符标注（古代女性文字）
- **说明**: 每行对应一条音频文件

#### `metadata_ipa_test.csv` (2.7 KB)
- **行数**: 29条记录
- **格式**: 测试集ID列表
- **用途**: 标记用于测试的样本，ID格式如 `p422-353`

#### `full_dataset.xlsx` (91 KB)
- **格式**: Excel工作簿
- **用途**: 完整数据集的汇总表，包含所有元数据
- **内容**: 合并了上述CSV文件的信息

## 🎵 音频文件

音频文件存储在 `wavs/` 文件夹中（需单独上传）：
- **总数**: 1,445个音频文件
- **格式**: `.wav` (WAVE audio, 16 bit mono 22050 Hz)
- **命名**: `pXXX-X.wav` (对应metadata中的ID)
- **特征**: `.spec.pt` PyTorch张量文件存储音频的光谱特征

## 📊 数据统计

| 指标 | 数值 |
|-----|------|
| 总样本数 | 1,445 |
| 测试集样本 | 29 |
| 训练集样本 | ~1,416 |
| 语言 | 江永方言 |
| 采样率 | 22050 Hz |
| 位深 | 16 bit |
| 声道 | 单声道 (mono) |

## 🏷️ 文件对应关系

```
音频ID: p19-2
├── 音频文件: wavs/p19-2.wav
├── 光谱特征: wavs/p19-2.spec.pt
├── 汉字: 奉到贵家拜龙门 (metadata_chinese.csv)
├── IPA标注: fang13 lau21 tciang33 kue44 poe21 liang42 mai42 (metadata_ipa.csv)
└── 女书标注: [...] (metadata_nvshu.csv)
```

## 🔍 使用说明

### 读取CSV数据
```python
import pandas as pd

# 读取汉字转录
chinese_df = pd.read_csv('metadata_chinese.csv', sep='|', header=None, 
                          names=['id', 'text', 'text_copy'])

# 读取IPA标注
ipa_df = pd.read_csv('metadata_ipa.csv', sep='|', header=None, 
                      names=['id', 'ipa', 'ipa_copy'])

# 合并数据
data = chinese_df.merge(ipa_df, on='id')
print(data.head())
```

### 访问音频文件
```python
import soundfile as sf

# 读取音频
audio, sr = sf.read('wavs/p19-2.wav')
print(f"采样率: {sr} Hz, 时长: {len(audio)/sr:.2f}s")

# 读取光谱特征
import torch
spec = torch.load('wavs/p19-2.spec.pt')
print(f"光谱特征形状: {spec.shape}")
```

## 📝 版本信息

- **数据集版本**: Final version (2025)
- **最后更新**: 2025年9月
- **数据来源**: 女书字典和江永方言语料库

## ⚖️ 使用许可

请查看LICENSE文件了解数据使用条款。

---

**说明**: 此数据集用于女书/江永方言语音识别和文本转语音(TTS)研究。
