# Nvshu-TTS Dataset

English | [中文](README_CN.md)

A text-to-speech (TTS) dataset for Nüshu and the Jiangyong dialect.

## 📁 File Description

### Core Data Files

#### `metadata_chinese.csv` (73 KB)
- **Rows**: 1,445 records
- **Format**: `ID|Chinese text|Chinese text`
- **Purpose**: Chinese character transcriptions corresponding one-to-one with the audio files
- **Example**:
  ```
  p19-2|奉到贵家拜龙门|奉到贵家拜龙门
  p19-4|一日之荣落贵家|一日之荣落贵家
  ```

#### `metadata_ipa.csv` (134 KB)
- **Rows**: 1,445 records
- **Format**: `ID|IPA transcription|IPA transcription`
- **Purpose**: IPA annotations for the Jiangyong dialect
- **Example**:
  ```
  p19-2|fang13 lau21 tciang33 kue44 poe21 liang42 mai42|fang13 lau21 tciang33 kue44 poe21 liang42 mai42
  ```

#### `metadata_nvshu.csv` (93 KB)
- **Rows**: 1,445 records
- **Format**: `ID|Nüshu annotation|Nüshu annotation`
- **Purpose**: Nüshu character annotations, representing an ancient women’s script
- **Note**: Each row corresponds to one audio file

#### `metadata_ipa_test.csv` (2.7 KB)
- **Rows**: 29 records
- **Format**: Test-set ID list
- **Purpose**: Marks samples used for testing, with IDs such as `p422-353`

#### `full_dataset.xlsx` (91 KB)
- **Format**: Excel workbook
- **Purpose**: A complete summary table of the dataset containing all metadata
- **Content**: Combines the information from the CSV files listed above

## 🎵 Audio Files

Audio files are stored in the `wavs/` directory and need to be uploaded separately:
- **Total**: 1,445 audio files
- **Format**: `.wav` (WAVE audio, 16-bit mono, 22050 Hz)
- **Naming**: `pXXX-X.wav`, corresponding to IDs in the metadata files
- **Features**: `.spec.pt` PyTorch tensor files store the spectral features of the audio

## 📊 Dataset Statistics

| Metric | Value |
|--------|-------|
| Total samples | 1,445 |
| Test samples | 29 |
| Training samples | ~1,416 |
| Language | Jiangyong dialect |
| Sampling rate | 22050 Hz |
| Bit depth | 16 bit |
| Channels | Mono |

## 🏷️ File Mapping

```
Audio ID: p19-2
├── Audio file: wavs/p19-2.wav
├── Spectral features: wavs/p19-2.spec.pt
├── Chinese text: 奉到贵家拜龙门 (metadata_chinese.csv)
├── IPA annotation: fang13 lau21 tciang33 kue44 poe21 liang42 mai42 (metadata_ipa.csv)
└── Nüshu annotation: [...] (metadata_nvshu.csv)
```

## 🔍 Usage

### Reading CSV Data
```python
import pandas as pd

# Read Chinese transcriptions
chinese_df = pd.read_csv(
    'metadata_chinese.csv',
    sep='|',
    header=None,
    names=['id', 'text', 'text_copy']
)

# Read IPA annotations
ipa_df = pd.read_csv(
    'metadata_ipa.csv',
    sep='|',
    header=None,
    names=['id', 'ipa', 'ipa_copy']
)

# Merge data
data = chinese_df.merge(ipa_df, on='id')
print(data.head())
```

### Accessing Audio Files
```python
import soundfile as sf

# Read audio
audio, sr = sf.read('wavs/p19-2.wav')
print(f"Sampling rate: {sr} Hz, duration: {len(audio) / sr:.2f}s")

# Read spectral features
import torch
spec = torch.load('wavs/p19-2.spec.pt')
print(f"Spectral feature shape: {spec.shape}")
```

## 📝 Version Information

- **Dataset version**: Final version (2025)
- **Last updated**: September 2025
- **Data sources**: Nüshu dictionary and Jiangyong dialect corpus

## ⚖️ License

Please see the LICENSE file for data usage terms.

---

**Note**: This dataset is intended for research on Nüshu/Jiangyong dialect speech recognition and text-to-speech (TTS).
