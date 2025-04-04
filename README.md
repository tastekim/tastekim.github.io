# 대선 게임 (Presidential Election Game)

<!-- 로고 이미지는 실제 프로젝트에 맞게 경로 수정 필요 -->
<!-- ![게임 로고](public/assets/logo.png) -->

## 소개

대선 게임은 실시간 투표와 전략적 의사결정을 통해 최후의 1인을 가리는 소셜 디덕션 게임입니다. 플레이어들은 각 라운드마다 다른 플레이어에게 투표하고, 후보자 등록 및 투표를 통해 정치적 전략을 펼치며 승리를 향해 나아갑니다.

## 목차

- [게임 특징](#게임-특징)
- [게임 진행 방식](#게임-진행-방식)
- [주요 기능](#주요-기능)
- [데이터 모델](#데이터-모델)
- [기술적 구현](#기술적-구현)
- [설치 및 실행](#설치-및-실행)
- [기술 스택](#기술-스택)
- [개발자 정보](#개발자-정보)

## 게임 특징

- **실시간 멀티플레이어**: Firebase 실시간 데이터베이스를 활용한 즉각적인 상호작용
- **투표 시스템**: 각 라운드마다 다른 플레이어에게 투표하여 결과 결정
- **후보자 등록 시스템**: 플레이어는 각 라운드마다 후보자로 등록할 수 있음
- **타이머 시스템**: 각 라운드는 제한 시간 내에 진행됨
- **재화 시스템**: 게임 내 골드와 다이아몬드를 획득하고 사용 가능
- **알림 기능**: 중요 게임 이벤트와 보상에 대한 실시간 알림
- **호스트 기능**: 방장은 게임 시작 및 플레이어 관리 권한 보유
- **다양한 애니메이션**: CSS와 Framer Motion을 활용한 다이나믹한 시각적 효과

## 게임 진행 방식

### 1. 게임 준비

1. 플레이어는 이름을 입력하고 게임방을 생성하거나 기존 게임에 참여합니다.
2. 최소 플레이어 수가 충족되면 호스트는 게임을 시작할 수 있습니다.
3. 각 플레이어는 게임 시작 시 일정량의 투표권을 받습니다.

### 2. 라운드 진행

1. **후보자 등록 단계**: 플레이어들은 후보자로 등록할 수 있습니다.
   - 각 라운드별로 최소 및 최대 후보자 수가 정해져 있습니다.
   - 후보자 등록 시 화려한 애니메이션이 표시됩니다.

2. **투표 단계**: 후보자 등록이 완료되면 플레이어들은 투표를 진행합니다.
   - 각 플레이어는 라운드당 정해진 수의 투표권을 가지며, 한 후보에게 여러 표를 던질 수 있습니다.
   - 투표는 제한 시간 내에 완료해야 합니다.
   - 투표 시 애니메이션 효과가 표시됩니다.

3. **결과 단계**: 라운드가 종료되면 다음과 같은 결과가 발생합니다.
   - 가장 많은 표를 받은 후보가 해당 라운드의 승자가 됩니다.
   - 승자와 패자에게 각각 다른 보상과 패널티가 주어집니다.
   - 결과 애니메이션을 통해 승자와 점수가 시각적으로 표시됩니다.

### 3. 게임 종료

1. 모든 라운드가 종료되면 최종 승자가 결정됩니다.
2. 게임 결과와 투표 기록이 모든 플레이어에게 표시됩니다.

## 주요 기능

### 게임 핵심 기능

#### 게임 세션 관리
- 게임 생성 및 참여
- 실시간 게임 상태 동기화
- 플레이어 상태 관리

#### 후보자 등록 시스템
- 후보자 등록 및 관리
- 후보자 등록 애니메이션

#### 투표 시스템
- 투표권 관리
- 투표 제출 및 계산
- 투표 애니메이션
- 라운드 결과 처리

#### 타이머 시스템
- 라운드별 시간 제한
- 자동 라운드 종료
- 남은 시간 표시

### 부가 기능

#### 재화 시스템
- 골드 및 다이아몬드 관리
- 호스트의 재화 지급 기능
- 재화 정보 표시

#### 알림 시스템
- 게임 이벤트 알림
- 재화 획득 알림
- 알림 읽음 처리 기능

#### 애니메이션 시스템
- CSS 및 Framer Motion 기반 애니메이션
- 후보자 등록 애니메이션
- 투표 캐스팅 애니메이션
- 라운드 결과 애니메이션

## 데이터 모델

게임의 핵심 데이터 모델은 다음과 같습니다:

### 게임 세션
```typescript
interface GameSession {
  id: string;                            // 게임 세션 고유 ID
  status: GameStatus;                   // 게임 상태 ("waiting" | "in-progress" | "finished")
  players: Record<string, Player>;       // 참여 플레이어 맵
  currentRound: number;                  // 현재 라운드 번호
  rounds: Record<number, Round>;         // 라운드 정보 맵
  settings: GameSettings;                // 게임 설정
  createdAt: Date;                       // 생성 시간
  updatedAt: Date;                       // 업데이트 시간
  winner: string | null;                 // 승리자 ID
}
```

### 플레이어
```typescript
interface Player {
  id: string;                            // 플레이어 고유 ID
  name: string;                          // 플레이어 이름
  isAlive: boolean;                      // 생존 여부
  votingHistory: Vote[];                 // 투표 기록
  remainingVotes: number;                // 남은 투표권 수
  isHost: boolean;                       // 호스트 여부
  isCandidate: boolean;                  // 후보자 여부
  currency: {                            // 재화 정보
    gold: number;
    diamond: number;
  };
  notifications: Record<string, Notification>;  // 알림 목록
}
```

### 라운드
```typescript
interface Round {
  number: number;                        // 라운드 번호
  status: RoundStatus;                   // 라운드 상태 ("candidate-selection" | "voting" | "completed")
  startTime: Date;                       // 시작 시간
  endTime?: Date;                        // 종료 시간
  votes: Record<string, RoundVote>;      // 투표 결과
  candidates: string[];                  // 후보자 목록
  eliminatedPlayer: string | null;       // 탈락한 플레이어 ID
  winner: string | null;                 // 라운드 승자 ID
}
```

### 투표
```typescript
interface Vote {
  from: string;                          // 투표한 플레이어 ID
  to: string;                            // 투표 대상 플레이어 ID
  amount: number;                        // 투표 수량
  round: number;                         // 라운드 번호
  timestamp: Date;                       // 투표 시간
}

interface RoundVote {
  to: string;                            // 투표 대상 플레이어 ID
  amount: number;                        // 투표 수량
}
```

### 게임 설정
```typescript
interface GameSettings {
  maxPlayers: number;                    // 최대 플레이어 수
  minPlayers: number;                    // 최소 플레이어 수
  votesPerPlayer: number;                // 플레이어당 투표권 수
  roundDurationSeconds: number;          // 라운드 제한 시간 (초)
  maxCandidates: number;                 // 최대 후보자 수
  minCandidates: number;                 // 최소 후보자 수
  maxRounds: number;                     // 최대 라운드 수
  candidateReward: number;               // 후보자 보상
  voterReward: number;                   // 투표자 보상
  candidatePenalty: number;              // 후보자 패널티
  voterPenalty: number;                  // 투표자 패널티
}
```

### 재화 및 알림
```typescript
interface Currency {
  amount: number;                        // 재화 수량
  type: "gold" | "diamond";              // 재화 유형
  timestamp: Date;                       // 지급 시간
}

interface Notification {
  id: string;                            // 알림 고유 ID
  message: string;                       // 알림 메시지
  read: boolean;                         // 읽음 여부
  timestamp: Date;                       // 생성 시간
  type: "currency" | "game" | "system";  // 알림 유형
  data?: object;                         // 추가 데이터
}
```

## 기술적 구현

### Firebase 데이터베이스 구조

게임 데이터는 Firebase Firestore에 다음과 같은 구조로 저장됩니다:

```
/games/{gameId}/ - 게임 세션
  - id
  - status
  - players
  - currentRound
  - rounds
  - settings
  - createdAt
  - updatedAt
  - winner
```

### 실시간 동기화

Firebase의 리얼타임 리스너를 사용하여 게임 상태 변경을 실시간으로 모든 플레이어에게 동기화합니다:

```typescript
subscribeToGame(gameId: string, callback: (game: GameSession) => void) {
  return onSnapshot(doc(db, GAMES_COLLECTION, gameId), (doc) => {
    if (doc.exists()) {
      callback(doc.data() as GameSession);
    }
  });
}
```

### 애니메이션 시스템

Framer Motion과 CSS를 활용한 애니메이션 시스템을 구현하여 역동적인 사용자 경험을 제공합니다:

```typescript
// 애니메이션 컨텍스트 및 관리자
export const AnimationProvider: React.FC<{ children: React.ReactNode }> = ({ children }) => {
  const [animationState, setAnimationState] = useState<AnimationState>(defaultAnimationState);
  
  // 투표 애니메이션
  const showVoteCastAnimation = (isWinner: boolean) => { ... };
  
  // 후보자 등록 애니메이션
  const showCandidateRegistrationAnimation = (playerName: string) => { ... };
  
  // 라운드 결과 애니메이션
  const showRoundResultAnimation = (players: Player[], winnerId: string | null, roundNumber: number) => { ... };
  
  return (
    <AnimationContext.Provider value={{ ... }}>
      {children}
      <AnimationRenderer />
    </AnimationContext.Provider>
  );
};
```

## 설치 및 실행

### 필요 조건

- Node.js 16.x 이상
- npm 또는 yarn

### 설치 방법

1. 저장소 클론:
```bash
git clone https://github.com/tastekim/tastekim.github.io.git
cd tastekim.github.io
```

2. 의존성 설치:
```bash
npm install
```

3. 개발 서버 실행:
```bash
npm start
```

4. 배포용 빌드:
```bash
npm run build
```

## 기술 스택

### 프론트엔드
- **React 18**: 사용자 인터페이스 구현
- **TypeScript**: 타입 안전성 확보
- **React Router 7**: 클라이언트 라우팅
- **Styled Components**: 컴포넌트 스타일링
- **Framer Motion**: 애니메이션 구현

### 백엔드
- **Firebase Firestore**: 실시간 데이터베이스
- **Firebase Authentication**: 사용자 인증

### 추가 라이브러리
- **UUID**: 고유 식별자 생성
- **React Icons**: 아이콘 컴포넌트

## 개발자 정보

- **개발자**: TasteKim
- **GitHub**: [github.com/tastekim](https://github.com/tastekim)
- **웹사이트**: [tastekim.github.io](https://tastekim.github.io)

---

© 2025 Presidential Election Game. All rights reserved.
