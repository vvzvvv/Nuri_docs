# Nuri

![Nuri](assets/nuri-main.png)

AI와의 음성 대화를 통해 한국어 회화를 연습하고, 문법·어휘·경어체(존댓말) 피드백을 받는 모바일 앱입니다. <br>
사용자는 음성으로 대화를 나누고, 대화 내용은 텍스트로 기록되며, 문법/어휘/경어체 관점의 피드백과 월별 대화 통계를 제공받습니다.

[Frontend Repo](https://github.com/vvzvvv/Nuri_frontend) | [Backend Repo](https://github.com/vvzvvv/Nuri_backend)



## Features

### 🎙️ 음성 대화
> 녹음한 음성을 STT로 변환해 AI와 대화, 응답은 TTS로 음성 재생

<img src="assets/screenshots/chat-with-ai.png" width="250" alt="대화"/>

### 👩🏻‍🏫 회화 피드백
> 문법 / 어휘 / 경어체(존댓말) 관점의 피드백 제공

<img src="assets/screenshots/feedbacks.png" width="750" alt="피드백"/>

### 💬 채팅 기록
> 대화 목록 및 히스토리 조회

<img src="assets/screenshots/chat-list.png" width="250" alt="채팅 목록"/>

### 🏠 홈 피드 (통계)
> 월별 대화 수, 대화 수 랭킹

<img src="assets/screenshots/home-feed.png" width="250" alt="홈"/>


## Tech Stack

**Backend**

![Spring Boot](https://img.shields.io/badge/Spring_Boot-6DB33F?style=for-the-badge&logo=springboot&logoColor=white)
![Java](https://img.shields.io/badge/Java-007396?style=for-the-badge&logo=openjdk&logoColor=white)
![Spring Security](https://img.shields.io/badge/Spring_Security-6DB33F?style=for-the-badge&logo=springsecurity&logoColor=white)
![JWT](https://img.shields.io/badge/JWT-000000?style=for-the-badge&logo=jsonwebtokens&logoColor=white)
![PostgreSQL](https://img.shields.io/badge/PostgreSQL-4169E1?style=for-the-badge&logo=postgresql&logoColor=white)
![MongoDB](https://img.shields.io/badge/MongoDB-47A248?style=for-the-badge&logo=mongodb&logoColor=white)
![AWS](https://img.shields.io/badge/AWS-232F3E?style=for-the-badge&logo=amazonaws&logoColor=white)
![OpenAI](https://img.shields.io/badge/OpenAI-412991?style=for-the-badge&logo=openai&logoColor=white)

**Frontend**

![React Native](https://img.shields.io/badge/React_Native-20232A?style=for-the-badge&logo=react&logoColor=61DAFB)
![React](https://img.shields.io/badge/React-61DAFB?style=for-the-badge&logo=react&logoColor=20232A)
![styled-components](https://img.shields.io/badge/styled--components-DB7093?style=for-the-badge&logo=styled-components&logoColor=white)

## Architecture


### 대화 처리 시퀀스
```mermaid
---
config:
  theme: base
---
sequenceDiagram
    participant FE as Front-End
    participant BE as Back-End
    participant PG as PostgreSQL
    participant Mongo as MongoDB
    participant STT as AWS Transcribe
    participant GPT as OpenAI API
    participant Polly as AWS Polly
    participant S3 as AWS S3

    FE->>BE: Web Socket 연결
    BE->>PG: Chat 세션 생성
    FE->>BE: 음성 데이터 전송
    BE->>S3: 사용자 음성 업로드
    BE->>STT: API 호출
    STT-->>BE: JSON 반환 (변환 내용)
    BE->>Mongo: ChatMessage 저장 (user)
    BE->>FE: 발화 데이터 전송
    BE->>GPT: API 호출
    GPT-->>BE: JSON 반환 (답변 내용)
    BE->>Mongo: ChatMessage 저장 (gpt)
    BE->>FE: 답변 텍스트 전송
    BE->>Polly: API 호출
    Polly->>S3: S3에 음성 데이터 저장
    S3-->>Polly: S3 URL 반환
    Polly-->>BE: S3 URL 반환
    BE->>FE: 답변 음성 데이터 전송
    FE->>BE: Web Socket 종료 요청
    BE->>PG: Ranking 갱신
    BE->>FE: 연결 종료
```
