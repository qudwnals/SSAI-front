# SSAI Frontend

React, TypeScript, Vite 기반의 CCTV 안전 관제 대시보드입니다. 개인 사용자와 기업 사용자가 시설·카메라를 관리하고, AI 위험 이벤트와 카메라 상태를 웹과 Android 앱에서 확인할 수 있습니다.

## 화면 미리보기

<p align="center">
  <a href="https://www.youtube.com/watch?v=O1-JNhcpvDQ">
    <img width="100%" alt="스마트 안전 관제 시스템 시연 영상" src="https://img.youtube.com/vi/O1-JNhcpvDQ/maxresdefault.jpg" />
  </a>
</p>

![실시간 AI 관제 및 사고 영상 검색](https://raw.githubusercontent.com/strangeRookies/.github/main/profile/assets/dashboard-and-search.jpg)
![사고 전후 이벤트 영상 재생](https://raw.githubusercontent.com/strangeRookies/.github/main/profile/assets/event-playback.jpg)

> 화면 이미지는 [strangeRookies 프로젝트 통합 자료](https://github.com/strangeRookies/.github/blob/main/profile/README.md)에서 재사용했습니다.

## 주요 기능

- 개인·기업 로그인과 회원가입
- SMS 인증, 비밀번호 재설정, 약관 동의 화면
- Access Token 메모리 관리와 세션 복구
- 시설·기업·카메라 목록 및 관리
- 카메라별 위험구역 ROI 설정 화면
- WebRTC, HLS, AI 오버레이 MJPEG 스트림 지원
- 카메라별 스트림 fallback과 stale 상태 처리
- STOMP 기반 위험 알림·카메라 상태·오버레이 구독
- 이벤트 ID 기반 알림 중복 제거와 최근 알림 관리
- 미확인 위험 알림 반복음과 확인(acknowledge) 처리
- 위험 이벤트 상세 화면과 카메라 포커스 이동
- Android Capacitor 앱과 FCM 백그라운드 푸시
- 푸시 알림 클릭 시 이벤트 상세 화면 이동
- VLM·시맨틱 검색 연동 화면과 이벤트 기록 기능

## 실시간 통신

```text
Spring Boot
  -> STOMP over WebSocket
  -> 시설·기업별 topic
  -> React alert feed / camera status / overlay store
```

프론트는 STOMP 연결 재연결, 이벤트 파싱, timestamp 기반 오버레이 동기화와 stale 이벤트 제거를 처리합니다. 코드에는 SSE EventSource 호환 경로도 있지만, 현재 백엔드의 주요 실시간 알림은 STOMP WebSocket을 사용합니다.

## 스트림 모드

```dotenv
VITE_STREAM_MODE=mjpeg
VITE_WEBRTC_BASE_URL=http://localhost:8889
VITE_HLS_BASE_URL=http://localhost:8888
VITE_MJPEG_BASE_URL=http://localhost:8010
VITE_STREAM_FALLBACK_ENABLED=true
```

- `webrtc`: MediaMTX WebRTC WHEP
- `raw`: MediaMTX HLS
- `mjpeg`: 카메라별 AI 오버레이 MJPEG
- 스트림 장애나 stale 상태 발생 시 설정된 fallback 경로 사용

## 실행

```bash
npm install
npm run dev
```

타입 검사와 빌드:

```bash
npm run typecheck
npm run build
```

Android 개발:

```bash
npm run android:sync
npm run android:open
```

운영 환경에서는 API와 스트림을 HTTPS/WSS로 제공하고, 개발용 mixed content 설정을 제거해야 합니다.

## 담당 개발 내용

- 개인·기업 로그인·회원가입 화면과 API 연동
- SMS 인증·약관 동의·비밀번호 재설정 UX
- 시설·카메라·ROI 설정 화면과 API 연결
- STOMP 실시간 이벤트·카메라 상태·오버레이 구독
- 이벤트 파싱, 중복 제거, stale 이벤트 정리와 알림음
- 위험 알림 확인 및 이벤트 상세·카메라 포커스 처리
- WebRTC/HLS/MJPEG 스트림 fallback과 오버레이 동기화
- Capacitor Android 앱과 FCM 기기 등록·푸시 클릭 처리

## 관련 저장소

- [SSAI-ai](https://github.com/qudwnals/SSAI-ai)
- [SSAI-back](https://github.com/qudwnals/SSAI-back)
