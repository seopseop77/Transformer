# Transformer Implementation for En-Ko Translation

이 저장소는 "Attention Is All You Need" (Vaswani et al., 2017) 논문에서 제안한 Transformer 모델을 PyTorch로 직접 구현하여 영어-한국어 번역을 수행하는 실습 프로젝트입니다.

모델의 구조(Encoder, Decoder, Multi-Head Attention 등)를 밑바닥부터(from scratch) 구현하여 아키텍처를 깊이 이해하는 것을 목표로 했습니다.

## Project Structure

전체 프로젝트 구조는 다음과 같습니다.

```text
├── main.ipynb                  # 메인 소스 코드 (전처리, 모델링, 학습, 추론) 
├── temp/
│   └── ProcessingData.ipynb    # AI Hub 데이터 가공 및 분할 스크립트
├── data/
│   ├── raw/                    # 가공된 학습 데이터가 위치해야 하는 곳
│   │   ├── train.en
│   │   ├── train.ko
│   │   ├── test.en
│   │   └── test.ko
│   └── spm/                    # 학습된 SentencePiece 모델 저장 경로
│       ├── spm.model
│       └── spm.vocab
├── ckpt/                       # 모델 체크포인트 저장소
└── requirements.txt            # 의존성 라이브러리 목록
```

## Dataset & Preprocessing

이 프로젝트는 저작권 문제로 인해 학습 데이터를 저장소에 직접 포함하지 않습니다. 모델 학습을 위해서는 아래 절차에 따라 데이터를 준비해야 합니다.

### Data Source
학습에는 **AI Hub(aihub.or.kr)**의 [한국어-영어 번역(병렬) 말뭉치] 데이터를 사용했습니다.
- 데이터셋 명: 한국어-영어 번역(병렬) 말뭉치 (구축년도: 2019)
- 출처: https://aihub.or.kr/aihubdata/data/view.do?currMenu=115&topMenu=100&aihubDataSe=data&dataSetSn=126

### Data Preparation
1. AI Hub에서 데이터를 다운로드합니다.
2. `temp/ProcessingData.ipynb`를 실행하여 원본 데이터를 정제하고 학습(Train) 및 평가(Test) 데이터로 분할합니다.
3. 생성된 `.en`, `.ko` 파일들을 `data/raw/` 디렉토리에 위치시킵니다. 데이터는 문장 단위로 줄바꿈 처리되어 있어야 합니다.

## Implementation Details

### 1. Environment & Tools
- **Language**: Python 3.12
- **Framework**: PyTorch
- **Tokenizer**: Google SentencePiece (BPE, Vocab Size: 16,000)

### 2. Model Architecture
- **Embedding**: Token Embedding + Sinusoidal Positional Encoding
- **Attention**: Scaled Dot-Product Attention, Multi-Head Attention
- **Normalization**: Pre-LN (Layer Normalization을 Sub-layer 이전에 적용하여 학습 안정성 확보)
- **Regularization**: Dropout (0.1)

### 3. Training Strategy
- **Optimizer**: AdamW
- **Scheduler**: Noam Scheduler (Warmup steps 적용)
- **Loss Function**: CrossEntropyLoss (Ignore Padding)

## How to Run

1. 환경 설정
   ```bash
   pip install -r requirements.txt
   ```
   
2. 데이터 준비
  위 'Dataset & Preprocessing' 항목을 참고하여 data/raw/ 경로에 데이터셋을 준비합니다.

3. 학습 및 테스트
  main.ipynb를 순차적으로 실행합니다. 노트북 내에서 SentencePiece 학습, 모델 초기화, 학습 루프, 최종 번역 테스트가 진행됩니다.

## References
Vaswani, A., et al. (2017). Attention Is All You Need. NeurIPS.

AI Hub 한국어-영어 번역(병렬) 말뭉치
