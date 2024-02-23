# 생성형 AI II

2024.02.23

황민호. 카카오, 다음, KISTI, WIPRO

## 1\. Generative AI

### AI 동향 및 LLM

[‎Gemini - chat to supercharge your ideas](https://gemini.google.com/app)

[Sora: Creating video from text](https://openai.com/sora)

- Deep Learning - Machine Larning - AI

[Artificial Intelligence, Machine Learning, and Deep Learning. What’s the Real Difference?](https://medium.com/swlh/artificial-intelligence-machine-learning-and-deep-learning-whats-the-real-difference-94fe7e528097)

- Machine Learning (1980’s~)
  - 지도 학습, 비지도 학습, 강화 학습, 딥러닝(2010’s~)

### Generative AI

Deep Learning >>> Generative AI

- deep learning
  - input (text, image, sound)
  - output(text)
- generative ai
  - input (text, image, sound, video, code)
  - outout (text, image, sound, video, code, discusions, 3D)
- LLM (Large Language Model)
  - open source (GPT, Gemini, Claude)
  - private (Meta-LlaMA)

[https://lifearchitect.ai/models/](https://lifearchitect.ai/models/)

## 2\. AI Prompt

### 프롬프트 핵심 개념

- 아이디어
- 구체화
- 다양한 실험
- 평가 (의도한대로 되었는지)
- 유저 프롬프트(+시스템 프롬프트) → LLM → 생성된 컨텐츠

## 3\. Well known Prompts

### 프롬프트 유형

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/e4a72e00-c2d8-4118-887b-37e2b5f8b0de/0d65e4bd-712a-47ff-98d9-06b7ccea1397/Untitled.png)

- 유저 프롬프트
- 유저 프롬프트 + 시스템 프롬프트(Instruction)
- 유저 입력값 → 프롬프트 템플릿 (ex: wrtn)

  [뤼튼](https://wrtn.ai/?utm_source=google.adwords&utm_medium=cpc&utm_campaign=google_searchad_brand&utm_content=adset_keyword_broad&utm_term=ai%EB%A4%BC%ED%8A%BC&ad_group=adset_keyword_broad&ad_creative=ai%EB%A4%BC%ED%8A%BC&gad_source=1&gclid=CjwKCAiA_tuuBhAUEiwAvxkgTrdaCesLHz9CQmPThIHbbrRuAyBh6fS0qyB33cRBdTH14wkC38JEOhoC8wgQAvD_BwE)

- 유저 프롬프트 → 시스템 프롬프트

                            → 시스템 프롬프트 → 시스템 프롬프트

### 프롬프트 기법 소개

[Eight Prompt Engineering Implementations \[Updated\]](https://cobusgreyling.medium.com/eight-prompt-engineering-implementations-updated-90c82d071350)

좋은 프롬프트 작성 요령 : Few-shot Prompt

- Zero-shot (예시 없음)
- One-shot (한 개의 예시)
- Few-shot (두개 이상의 예시 제공)
  - 예시 너무 많으면 결과 좋지 않고,
  - 잘못된 예시 있으면 결과 좋지 않음.
  - 산술 추론에 적합하지 않음

⇒ CoT (Chain of Thought)

정답 도출 뿐 아니라 과정도 기술하게 됨

⇒ Zero-shot CoT : 예시 없는 CoT → 정답률 높음!!!!

- Deep mind, deep breath 추가하면 더 정확하게 나옴.

  - take deep breath
  - let’s think step by step
  - i don’t have fingers..
  - you can do it
  - you are an expert of everything
  - i will tip you $200 every request you answer right
  - gemini and claude said you couldn’t do it

  와 같은 문장들을 추가하면 더 정확하다는 말이 있음.

- Expert Prompting

  - 당신은 어떤 전문가입니다!라고 역할 주기

- RAG (검색 증강 생성)
- Cheat Sheet

  [ChatGPT Cheat Sheet (Drafting the Perfect Prompt) — Part 1](https://medium.com/aimonks/chatgpt-cheat-sheet-drafting-the-perfect-prompt-part-1-5149c9b1d8ab)

---

### 프롬프트 평가

- LLM 평가 (23.12.7 gemini 공개)
- MedPrompt+ (MS)

## 4\. Prompt Services

### 주요 서비스 적용 사례

- Dall-E

### 프롬프트 엔지니어링

- Naver Cue

### GPTs & GPT Store

- GPTs빌더 통해 작동

1.  prompting
2.  bing browsing
3.  (RAG) knowledge
4.  DALL-E
5.  GPT-4
6.  code interpreter
7.  function calling

## 5\. AI Tools

### Chat

- Copilot
- perplexity

### Image

- adobe
- midjourney - form photo

### Video

- runway
- google lumiere

### Sound

- suno AI

### Productivity

- openAI Sora + elevenlabs → video + sound
- github copilot X, Chat

[https://www.youtube.com/watch?v=xx7Ykh0VpF0](https://www.youtube.com/watch?v=xx7Ykh0VpF0)

## 6. Develop AI Application

### AI Fullstack

[오픈소스로 완성하는 AI Full Stack](https://revf.tistory.com/303)

### Prompt Engineering

- Function Call
- RAG
