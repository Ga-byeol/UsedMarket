# 🍆 가지마켓 (GajiMarket) - P2P 중고거래 플랫폼

> "나가지말고 집에서 편하게 중고거래"

<br>

## 📋 프로젝트 소개

**가지마켓**은 개인 간(P2P) 중고 물품을 등록하고 거래할 수 있는 웹 기반 중고거래 플랫폼입니다.  
당근마켓, 번개장터와 같은 서비스를 벤치마킹하여 상품 등록·검색·찜하기, 실시간 채팅, 거래 후기, 자유게시판, 고객센터 등의 기능을 구현하였습니다.

<br>

## 🗓 개발 기간 & 팀 구성

| 항목 | 내용 |
|------|------|
| **개발 기간** | 2023.02.01 ~ 2023.03.17 (약 7주) |
| **팀 구성** | 6명 (아이티윌 부산캠퍼스 파이널 프로젝트 1조) |

| 이름 | 역할 |
|------|------|
| 김도연 | 조장, 상품 상세/목록, 찜(좋아요), 메인 페이지 |
| **최승현** | **프론트엔드 레이아웃(Header/Footer/Hero), 자유게시판, 실시간 채팅** |
| 양성일 | 상품 등록/수정/삭제, 공지사항 |
| 고보정 | 고객센터(Q&A), 댓글 |
| 한도경 | 판매/구매 내역, 매너 후기 |
| 조보규 | 회원(가입/로그인/마이페이지), 결제 연동 |

<br>

## ⚙️ 기술 스택

### Backend
| 기술 | 버전 |
|------|------|
| Java | 11 |
| Spring MVC | 4.3.8 |
| MyBatis | 3.4.1 |
| MySQL | 8.0 |
| Apache Tomcat | 9.0 |
| Maven | - |

### Frontend
| 기술 | 설명 |
|------|------|
| JSP / JSTL | 서버사이드 뷰 렌더링 |
| Bootstrap 4 | 반응형 레이아웃 |
| jQuery 3.x | DOM 조작 및 AJAX 통신 |
| JavaScript (ES5) | 클라이언트 로직 |
| Owl Carousel | 슬라이더/캐러셀 |

### Communication
| 기술 | 설명 |
|------|------|
| WebSocket + STOMP | 실시간 1:1 채팅 |
| Jackson / Gson | JSON 직렬화/역직렬화 |

### Infra & Collaboration
| 도구 | 용도 |
|------|------|
| GitHub | 버전관리 및 Pull Request 기반 협업 |
| STS (Spring Tool Suite) | IDE |
| Daum Postcode API | 주소 검색 |

<br>

## 🏗 시스템 아키텍처

```
Client (Browser)
    │
    ├── HTTP Request ──▶ DispatcherServlet
    │                         │
    │                    Controller (@Controller / @RestController)
    │                         │
    │                    Service (@Service)
    │                         │
    │                    DAO (@Repository) ──▶ MyBatis SqlSession
    │                         │
    │                    MySQL Database
    │
    └── WebSocket/STOMP ──▶ ChatRoomController (@MessageMapping)
                              │
                         ChatService ──▶ txt 파일 기반 메시지 저장
```

<br>

## 📁 프로젝트 구조

```
Final_Project/
├── src/main/java/com/itwillbs/
│   ├── controller/          # 컨트롤러 (MVC 요청 처리)
│   │   ├── MainController        # 메인 페이지
│   │   ├── MemberController      # 회원 관리
│   │   ├── ProductController     # 상품 CRUD
│   │   ├── FreeBoardController   # 자유게시판 ★
│   │   ├── ChatRoomController    # 실시간 채팅 ★
│   │   ├── ChatAjaxController    # 채팅 AJAX API ★
│   │   ├── WishController        # 찜하기
│   │   ├── ReviewController      # 거래 후기
│   │   └── ...
│   ├── service/             # 비즈니스 로직
│   │   ├── ChatService           # 채팅 서비스 (파일 I/O) ★
│   │   ├── FreeBoardService      # 자유게시판 서비스 ★
│   │   └── ...
│   ├── dao/                 # 데이터 접근 계층
│   └── domain/              # DTO (Data Transfer Object)
│
├── src/main/resources/
│   ├── database.properties       # DB 접속 정보 (외부화)
│   ├── mybatis-config.xml        # MyBatis 설정
│   └── mappers/                  # SQL Mapper XML
│       ├── freeboardMapper.xml   # 자유게시판 SQL ★
│       ├── chatMapper.xml        # 채팅 SQL ★
│       └── ...
│
├── src/main/webapp/
│   ├── resources/
│   │   ├── fragments/            # 공통 레이아웃 ★
│   │   │   ├── header.jsp        # 헤더 ★
│   │   │   ├── footer.jsp        # 푸터 ★
│   │   │   └── hero.jsp          # 히어로 배너 ★
│   │   ├── css/
│   │   ├── js/
│   │   ├── upload/               # 상품 이미지 업로드
│   │   └── txt/                  # 채팅 기록 저장
│   │
│   └── WEB-INF/
│       ├── views/
│       │   ├── main/             # 메인 페이지
│       │   ├── member/           # 회원 관련 뷰
│       │   ├── product/          # 상품 관련 뷰
│       │   ├── freeboard/        # 자유게시판 뷰 ★
│       │   ├── chat/             # 채팅 뷰 ★
│       │   └── ...
│       └── spring/               # Spring 설정
│           ├── root-context.xml
│           └── appServlet/servlet-context.xml
│
└── pom.xml                       # Maven 의존성 관리
```

> ★ = 최승현 담당 파트

<br>

## 🖥 주요 기능 및 화면

### 1. 메인 페이지
- 카테고리별 상품 탐색 (여성의류, 남성의류, 신발/가방 등 11개 카테고리)
- 조회순 / 관심순 / 최신등록순 상품 캐러셀 슬라이더
- 통합 검색 기능

### 2. 회원 기능
- 회원가입 (약관 동의 → 정보 입력 → Daum 주소 API)
- 로그인 / 로그아웃
- 마이페이지 (프로필 수정, 매너 점수 확인)
- 아이디/비밀번호 찾기 (임시 비밀번호 발급)

### 3. 상품 관리
- 상품 등록 (최대 6장 이미지 업로드, 미리보기, 카테고리/상태/가격 설정)
- 상품 목록 (카테고리 필터, 가격 범위 필터, 정렬, AJAX 무한 스크롤)
- 상품 상세 (이미지 갤러리, 판매자 다른 상품 추천)
- 찜하기 (좋아요 토글, 찜 목록 관리)

### 4. 자유게시판 ⭐ (담당 기능)
- 게시글 CRUD (작성 / 조회 / 수정 / 삭제)
- 댓글 시스템 (AJAX 비동기 댓글 작성/삭제)
- 페이징 처리 (10개 단위 페이지 블록)
- 검색 기능 (제목 기반 검색)
- 좋아요 기능

### 5. 실시간 채팅 ⭐ (담당 기능)
- **WebSocket + STOMP 프로토콜** 기반 1:1 실시간 채팅
- 상품 상세 페이지에서 "채팅하기" 클릭 시 채팅방 자동 생성
- 이전 대화 내역 조회 (txt 파일 기반 영구 저장)
- 채팅방 목록 관리 (내 채팅방 조회, 나가기)
- 실시간 메시지 송수신 (SimpMessagingTemplate)

### 6. 공통 레이아웃 ⭐ (담당 기능)
- **Header** - 네비게이션 바, 카테고리 드롭다운, 로그인 상태 표시
- **Footer** - 사이트 정보, 연락처
- **Hero Section** - 메인 배너, 카테고리 사이드바

### 7. 고객센터 (Q&A)
- 게시글 CRUD + 다중 첨부파일 업로드 (최대 3개)
- 비밀글 기능 (관리자 및 작성자만 열람 가능)
- 댓글 / 대댓글 시스템
- 처리 상태 관리 (관리자 전용)
- 검색 + 페이징

### 8. 기타
- 공지사항 (관리자 전용 CRUD, 이미지 첨부)
- 판매/구매 내역 관리
- 거래 후기 & 매너 점수 시스템
- 오픈뱅킹 API 연동 (결제 프로토타입)

<br>

## 💾 데이터베이스 설계 (ERD)

프로젝트는 총 **11개 테이블**로 구성됩니다:

| 테이블명 | 설명 |
|----------|------|
| `members` | 회원 정보 (ID, 비밀번호, 닉네임, 주소, 매너점수 등) |
| `product` | 상품 정보 (제목, 가격, 카테고리, 이미지, 상태 등) |
| `categories` | 상품 카테고리 |
| `chat` | 채팅방 정보 (구매자, 판매자, 상품번호) |
| `freeboard` | 자유게시판 |
| `freeboard_comm` | 자유게시판 댓글 |
| `customerservice` | 고객센터 게시판 |
| `cs_comment` | 고객센터 댓글 |
| `notice` | 공지사항 |
| `buylist` | 구매 내역 |
| `reviews` | 거래 후기 |

<br>

## 🔧 환경 설정 및 실행 방법

### 1. 사전 요구사항
- JDK 11+
- Apache Tomcat 9.0
- MySQL 8.0
- STS (Spring Tool Suite) 또는 Eclipse

### 2. DB 설정
`src/main/resources/database.properties` 파일을 환경에 맞게 수정하세요:
```properties
db.driver=com.mysql.cj.jdbc.Driver
db.url=jdbc:mysql://localhost:3306/gajimarket?serverTimezone=Asia/Seoul
db.username=root
db.password=your_password
```

### 3. 실행
1. MySQL에 데이터베이스 및 테이블 생성
2. STS에서 프로젝트 Import (`Existing Maven Projects`)
3. Tomcat 서버 연동 후 Run
4. 브라우저에서 `http://localhost:8080/market/main/main` 접속

<br>

## 📝 담당 파트 상세 설명

### 🟣 자유게시판 (Free Board)

**구현 기능:**
- Spring MVC 패턴에 따른 게시글 CRUD
- AJAX 기반 비동기 댓글 시스템 (`@RestController`)
- 커스텀 페이징 처리 (PageDTO 활용, 10개 블록 단위)
- 검색 기능 (MyBatis 동적 SQL `<if>` 조건)

**관련 파일:**
| 레이어 | 파일 |
|--------|------|
| Controller | `FreeBoardController.java`, `FreeBoardAjaxController.java` |
| Service | `FreeBoardService.java`, `FreeBoardServiceImpl.java` |
| DAO | `FreeBoardDAO.java`, `FreeBoardDAOImpl.java` |
| Domain | `FreeBoardDTO.java`, `FreeBoardCommDTO.java`, `PageDTO.java` |
| Mapper | `freeboardMapper.xml` |
| View | `freeboard/freeboard.jsp`, `content.jsp`, `writeForm.jsp`, `updateForm.jsp` |

---

### 🟣 실시간 채팅 (Real-time Chat)

**구현 기능:**
- Spring WebSocket + STOMP 프로토콜을 활용한 1:1 실시간 채팅
- 채팅방 자동 생성: 상품 상세에서 "채팅하기" 클릭 시 기존 채팅방 조회 → 없으면 새로 생성
- txt 파일 기반 채팅 이력 저장 및 조회 (FileOutputStream / BufferedReader)
- `@MessageMapping` + `SimpMessagingTemplate`으로 실시간 메시지 라우팅

**동작 흐름:**
```
1. 구매자가 상품 상세에서 "채팅하기" 클릭
2. AJAX POST /chat/room → 채팅방 조회/생성
3. 채팅방 입장 → WebSocket 연결 (/user/chat/room/{roomId} 구독)
4. 메시지 전송 → @MessageMapping("/chat/message") → SimpMessagingTemplate 라우팅
5. 메시지 txt 파일 저장 (sender\nmessage\nsendTime 포맷)
6. 채팅방 재입장 시 txt 파일 읽어 이전 대화 표시
```

**관련 파일:**
| 레이어 | 파일 |
|--------|------|
| Controller | `ChatRoomController.java`, `ChatAjaxController.java` |
| Service | `ChatService.java` |
| DAO | `ChatDAO.java`, `ChatDAOImpl.java` |
| Domain | `ChatRoomDTO.java`, `MessageDTO.java` |
| Mapper | `chatMapper.xml` |
| View | `chat/room.jsp`, `chat/roomdetail.jsp` |

---

### 🟣 공통 레이아웃 (Header / Footer / Hero)

**구현 기능:**
- `header.jsp` : 사이트 로고, 네비게이션 메뉴, 카테고리 드롭다운, 검색바, 로그인/회원가입 버튼
- `footer.jsp` : 사이트 정보 및 소셜 미디어 링크
- `hero.jsp` : 메인 히어로 배너 + 카테고리 사이드바
- JSP `include` 방식으로 모든 페이지에서 재사용

<br>

## 🛠 트러블슈팅 & 학습 포인트

| 문제 | 해결 방법 |
|------|----------|
| WebSocket 연결 끊김 | STOMP 프로토콜 도입으로 안정적인 메시지 라우팅 구현 |
| 채팅 이력 관리 | DB 대신 txt 파일 I/O 방식으로 경량화, FileOutputStream append 모드 활용 |
| 6인 동시 협업 충돌 | GitHub Fork → Pull Request 워크플로우 도입, 회의록 기반 작업 분배 |
| 파일 업로드 경로 호환 | 하드코딩 절대경로 → `servletContext.getRealPath()` 동적 경로로 개선 |

<br>

## 📊 Git 기여도

- **전체 커밋 수**: 100+ (팀 전체)
- **최승현 주요 커밋**: 21건
  - `header & footer update/create`
  - `hero section create`
  - `freeboard CRUD & update`
  - `chat 90percent → chat update`

<br>

## 📌 개선 사항 (리팩토링 완료)

| 항목 | Before | After |
|------|--------|-------|
| DB 접속 정보 | XML에 평문 하드코딩 | `database.properties` 외부 파일 분리 |
| 파일 업로드 경로 | 개발자별 로컬 절대경로 4개 | `servletContext.getRealPath()` 동적 경로 |
| 디버그 로깅 | `System.out.println()` | SLF4J Logger |
| 인코딩 필터 | `forceEncoding` 미설정 | `forceEncoding=true` 추가 |
| 의존성 관리 | jackson-databind 중복 선언 | 단일 버전 통합 |
| .gitignore | 기본 템플릿만 | target, IDE, 업로드 디렉토리 등 포괄적 규칙 |
| 미사용 import | protobuf 등 컴파일 에러 유발 | 정리 완료 |

<br>

---

**© 2023 가지마켓 1조 | 아이티윌 부산캠퍼스 파이널 프로젝트**
