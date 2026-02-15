# JStudy 

SwiftUI 기반으로 제작한 일본어 학습 앱입니다.  
듣기, 받아쓰기, 문장 배열, 따라 말하기, 어휘 복습까지 하나의 학습 플로우로 연결해 학습 몰입도를 높임. 

## 프로젝트 요약

- **프로젝트명**: JStudy
- **플랫폼**: iOS (SwiftUI)
- **목표**: 일본어 학습을 단계형 UX로 구성해, 입력/출력(듣기-말하기-쓰기) 학습을 한 앱에서 수행
- **핵심 구현 범위**:
  - 소셜 로그인(카카오/구글) 및 세션 복구
  - 5단계 학습 플로우 설계 및 상태 전환 제어
  - 음성 인식(STT) 기반 발화 정확도 계산
  - 단어 북마크/복습 데이터 영구 저장 및 실시간 동기화

## 주요 기능

1. **온보딩 및 인증**
- Kakao / Google 로그인 지원
- 앱 재실행 및 포그라운드 복귀 시 세션 자동 점검/복구
- 로그인 상태 점검 중 스플래시 처리로 화면 깜빡임 방지

2. **5-Step 학습 플로우**
- Step 1: 영상 기반 집중 듣기
- Step 2: 받아쓰기 객관식(정답/오답 피드백 + 햅틱)
- Step 3: 드래그 앤 드롭 문장 완성
- Step 4: 따라 말하기(STT 인식 문장 + 정확도 점수)
- Step 5: 어휘/문법 학습 및 북마크 저장

3. **오늘의 회화**
- 대화 말풍선 순차 노출 애니메이션
- 일본어 TTS 재생
- 핵심 단어 시트 연동 및 단어장 저장

4. **나만의 단어장**
- 학습/회화 탭 분리 관리
- 전체 선택, 일괄 삭제, 중복 제거
- UserDefaults(JSON 인코딩) 기반 영구 저장

5. **가나(히라가나/가타카나) 학습**
- 5열 그리드 테이블
- 문자 상세 팝업(행 정보, 발음, 획순)
- AVSpeechSynthesizer 기반 발음 재생

## 기술 스택

- **Language/UI**: Swift 5, SwiftUI
- **Auth/Backend SDK**: KakaoSDK, GoogleSignIn, Firebase
- **Media/Voice**: AVKit, AVFoundation, Speech
- **Animation**: Lottie
- **Persistence**: UserDefaults + JSONEncoder/Decoder
- **State/Event**: ObservableObject, NotificationCenter
- **Dependency**: Swift Package Manager

## 구현 포인트 

1. **세션 복구 UX 안정화**
- `AppState`에서 `isChecking` 상태를 분리해, 인증 상태 확인 전 화면 노출을 제어
- 앱 시작/활성화 시 세션 재검증으로 로그인 흐름 안정화

2. **멀티 스텝 영상 재생 충돌 방지**
- `PlayerViewModel`에 `forcePause`, `stopAndReset`, `playFromStart`를 분리
- Step 전환/이탈 시점마다 재생 상태를 명시적으로 제어해 오디오 중첩 이슈 방지

3. **STT 정확도 산출 로직 구현**
- 일본어 텍스트 정규화(전각/반각, 가나 폴딩, 기호 제거) 후 문자 정렬
- Levenshtein 기반 정렬 결과로 정확도(%) 계산

4. **학습 데이터 동기화**
- 북마크 변경 이벤트를 `NotificationCenter`로 발행/구독
- 학습 화면과 단어장 화면 간 실시간 반영, 저장 시 중복 제거 로직 적용

## 코드 구조

```text
일본어/
├─ App/                  # App 진입점, AppState, AppDelegate
├─ Navigation/           # Main TabView
├─ Onboarding/           # 로그인 화면
├─ Home/                 # 홈 대시보드
├─ Learning Screens/
│  ├─ LessonCardView/    # 5-Step 학습 플로우
│  ├─ ConversationView/  # 오늘의 회화
│  ├─ VocabularyView/    # 단어장
│  ├─ ReviewView/        # 복습 화면
│  └─ KanaSectionView/   # 가나 학습
├─ Models/               # PlayerViewModel, SpeechRecognizer, 데이터 모델
└─ Resources/            # Assets, Color Theme
```

## 실행 방법

1. Xcode에서 `/Users/eomjiyong/Desktop/Japanese/일본어.xcodeproj` 열기
2. Swift Package 의존성 로딩 확인
3. `GoogleService-Info.plist` 설정 확인
4. Kakao 앱 키 설정 확인
5. 시뮬레이터 또는 실기기에서 빌드/실행

