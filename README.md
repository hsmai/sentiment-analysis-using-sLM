# sentiment-analysis-using-sLM

NSMC(네이버 영화 리뷰 감성분석 데이터셋)에 대해 llama-3.2-Korean-Bllossom-3B 모델을 LoRA fine tuning하여 데이터셋에 대한 성능을 확인합니다.

[BaseLine]
- no fine tuning
- 1B Llama3.2 모델
- 영어 프롬프트
- 토큰 확률 기반 분류
- zero-shot prompt

✅ accuracy: 0.686

---

## 주요 구현 사항

- NSMC에 대해 Fine-tuning
- 다양한 LoRA 설정으로 실험
- Few-shot prompt 적용
- Reasoning 데이터셋 생성 및 학습

---

## 사용 환경

- Python 3.8+
- 필수 라이브러리:  
  `torch`, `unsloth`, `datasets`, `trl`, `transformers`, `tqdm` 등

노트북은 [Google Colab](https://colab.research.google.com/) 또는 로컬 Jupyter 환경에서 실행 가능합니다.

---

## 1. NSMC에 대해 Fine-tuning

- 3B 한국어 Llama3.2 모델(Bllossom) 로 변경
- NSMC 데이터셋 샘플링하여(1,000) 적용
- 한국어 프롬프트로 변경
- 확률 분석 대상 토큰 변경

✅ accuracy: 0.6980<br><br>

&lt;main point&gt;
- 1B -> 3B 파라미터 개수 증가 영향으로 성능 개선 확인
- 기존 영어 기반에서 프롬프트 및 토큰 변경을 통해 한국어 기반으로 수정

---

## 2. 다양한 LoRA 설정으로 실험

- 튜닝 별 성능 비교
  
[baseline] (no fine tuning)

✅ accuracy: 0.6980<br><br>


[r=4]

✅ accuracy: 0.7460<br><br>


[r=2]

✅ accuracy: 0.7210<br><br>

&lt;main point&gt;
- 다양한 LoRA 설정을 실험하여 rank가 미치는 영향 확인
- rank 증가 시 정확도 향상 경향성 확인

---

## 3. Few-shot prompt 적용

- few-shot prompt로 변경
- 2개의 demonstration을 추가

✅ accuracy: 0.6240<br><br>

&lt;main point&gt;
- demonstration을 추가한 few-shot prompt로 변경 후 성능 하락
- 잘못된 예시로 인한 성능 하락으로 보여짐

---

## 4. Reasoning data 생성 및 학습

- API를 활용하여 Gemini 2.5 flash 모델로 1,000개 NSMC 데이터에 대해 reasoning(근거) 데이터 생성
- NSMC 데이터와 생성한 reasoning 데이터를 연결하여 (문제-정답-근거) 순으로 데이터셋 구성
- SFT 방식으로 sLM 학습 후 분류 성능 평가

✅ accuracy: 0.7090<br><br>

&lt;main point&gt;
- reasoning(근거)을 데이터에 추가 후 오히려 성능 하락
- 충분하지 못한 데이터 개수, 생성된 reasoning 데이터 자체의 품질 문제 등으로 인해 정확도가 하락한 것으로 보여짐
