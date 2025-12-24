# MagNavi  
지구 자기장과 스마트폰 센서를 활용한 초정밀 실내 내비게이션 시스템
<img width="1280" height="720" alt="슬라이드1" src="https://github.com/user-attachments/assets/b93510ca-e492-4b41-8f0d-0e4472d5ebc6" />

<br>

## 📝 프로젝트 소개  
<img width="1280" height="720" alt="슬라이드3" src="https://github.com/user-attachments/assets/7393084d-1d82-4faf-96db-c7e0732d67da" /><br>

실내 공간에서는 GPS 신호 수신이 불가능하여 위치 기반 서비스(LBS)의 제공에 제약이 있습니다. 기존 Wi-Fi, BLE 비콘 기반 실내 측위 기술은 추가 인프라 설치와 유지 비용이 크고, 전파 신호 특성상 3~20m 수준의 큰 오차가 발생하는 한계가 있습니다.  

**MagNavi**는 스마트폰 내장 센서(자기장, 가속도, 자이로스코프 등)만을 활용하여 지구 자기장의 고유 분포 패턴을 학습하고, 보행자 추측 항법(PDR)과 딥러닝 기반 모델을 결합함으로써 **1m 이하 수준의 실내 측위 정확도**를 목표로 합니다.  

또한 교통약자를 포함한 다양한 사용자 유형을 고려하여, 휠체어 사용자에게는 계단을 배제한 경로를, 저시력 사용자에게는 색상 및 음성 안내 최적화 기능을 제공하는 등 **사용자 맞춤형 실내 내비게이션**을 구현합니다.  
<br>

## 📆 개발 기간  
1차. *2025.03 – 2024.06* : 센서 데이터 수집 및 MLP 기반 초기 모델 구현  
2차. *2025.09 – 현재* : PDR 기반 이동 보정 알고리즘 및 실시간 위치 추정 앱 개발    
<br>

## 🧑‍💻 팀 구성  
- 백 : 윤찬익, 박윤호
- 프론트 : 류효정, 이재훈, 이준규
- AI : 류효정, 박윤호, 염수민
<br>

## ⚙️ 개발 환경  
- **언어** : `Python 3.10+`, `Dart (Flutter)`, `Kotlin (Jetpack Compose)`  
- **IDE** : `VS Code`, `Android Studio`  
- **Framework / Library** : `PyTorch`, `scikit-learn`, `FastAPI`, `Flutter`, `Jetpack Compose`  
- **Database** : SQLite (POI 및 공간 데이터 관리)  
- **협업 도구** : GitHub, Figma  
<br>

## 📌 주요 기능  

### 1. 지자기 기반 실내 측위  
- 스마트폰 자기장 센서(Mag_X, Mag_Y, Mag_Z) 및 오리엔테이션(Ori_X, Ori_Y, Ori_Z) 데이터 활용  
- Fingerprint + MLP 분류 모델 기반 초기 위치 추정 (정확도 최대 60%)  
- LSTM/RNN 기반 시퀀스 데이터 학습을 통한 초정밀 위치 추정  

### 2. PDR (Pedestrian Dead Reckoning) 보정  
- 가속도/자이로스코프 기반 걸음 수 검출 및 보폭 추정  
- 방향 보정(자기장·자이로 융합) 및 Δx, Δy 이동 거리 산출  
- 위치 예측 모델의 입력 피처로 활용  

### 3. 교통약자 맞춤형 내비게이션  
- 휠체어 사용자 : 계단 배제, 직선 구간 위주 안내  
- 저시력 사용자 : 다크/화이트 모드 지원, 음성 안내 제공  
- 일반 사용자 : 최단 경로 기반 실시간 길찾기  

### 4. 실내 맵 및 경로 안내  
- CAD 도면 및 현장 답사 기반 POI 데이터베이스 구축  
- A* 알고리즘 기반 최단 경로 탐색  
- Flutter/Jetpack Compose 기반 모바일 UI 구현  
<br>

## 📊 성능 및 결과  
- Wi-Fi/BLE 방식 대비 **5~10배 높은 정확도** 달성 (평균 오차 약 0.7m, KOLAS 시험 인증)  
- 교통약자 이동 시간 개선 효과 :  
  - 휠체어 사용자 **53% 단축**  
  - 저시력 사용자 **47% 단축**  
- 초기 위치 인식 속도 : 4걸음 이내 위치 고정, 평균 오차 66cm  
<br>

## 📐 시스템 구조도  
아래는 MagNavi 시스템 구조 예시입니다.  

![System Diagram](./images/system_diagram.png)  
*Fig.1 시스템 아키텍처*  



[MagNavi ERD]
![MagNavi DB 구조](https://github.com/midas-capston-design/back_fastapi/blob/main/image/magnavi_db.png?raw=true)
전체 테이블 요약

users: 사용자 계정 정보를 저장하는 테이블

favorites: 각 사용자의 즐겨찾기 정보를 저장하는 테이블

predicted_locations: 실내 위치 예측 모델이 인식하는 장소 정보를 저장하는 테이블

outdoor_place: 외부 장소의 상세 정보를 저장하는 테이블

## 1. `users`

> 사용자 계정 정보를 저장하는 테이블입니다. 애플리케이션의 인증 및 사용자 식별에 사용됩니다.

-   **`id`** (`INTEGER`, **PK**, `AUTO_INCREMENT`)
    -   사용자의 고유 식별 번호 (시스템 내부용).
-   **`userId`** (`VARCHAR(50)`, **UNIQUE**, `NOT NULL`)
    -   사용자가 로그인 시 사용하는 고유 아이디.
-   **`userName`** (`VARCHAR(50)`, `NOT NULL`)
    -   사용자의 이름 또는 닉네임.
-   **`email`** (`VARCHAR(100)`, **UNIQUE**, `NOT NULL`)
    -   사용자의 이메일 주소.
-   **`phone_number`** (`VARCHAR(20)`, `NOT NULL`)
    -   사용자의 전화번호.
-   **`hashed_password`** (`VARCHAR(255)`, `NOT NULL`)
    -   보안을 위해 bcrypt로 암호화된 사용자 비밀번호.

---

## 2. `favorites`

> 각 사용자가 등록한 즐겨찾기 목록을 저장합니다. `users` 테이블과 1:N 관계를 가집니다.

-   **`id`** (`VARCHAR(255)`, **PK**)
    -   즐겨찾기 항목의 고유 ID (클라이언트에서 생성).
-   **`user_id`** (`INTEGER`, **FK**, `NOT NULL`)
    -   이 즐겨찾기를 소유한 사용자의 `id` (`users` 테이블 참조).
-   **`type`** (`ENUM`, `NOT NULL`)
    -   즐겨찾기의 종류. (`'place'`, `'bus'`, `'busStop'`)
-   **`name`** (`VARCHAR(255)`, `NOT NULL`)
    -   즐겨찾기에 부여된 이름 (예: "우리집", "814번 버스").
-   **`created_at`** (`TIMESTAMP`, `DEFAULT CURRENT_TIMESTAMP`)
    -   즐겨찾기 생성 일시.
-   **`address`** (`VARCHAR(255)`)
    -   `type`이 `'place'`일 경우의 주소.
-   **`place_category`** (`ENUM`)
    -   `type`이 `'place'`일 경우의 카테고리. (`'home'`, `'work'`, `'etc'`)
-   **`bus_number`** (`VARCHAR(50)`)
    -   `type`이 `'bus'`일 경우의 버스 번호.
-   **`station_name`** (`VARCHAR(255)`)
    -   `type`이 `'busStop'`일 경우의 정류장 이름.
-   **`station_id`** (`VARCHAR(255)`)
    -   `type`이 `'busStop'`일 경우의 정류장 고유 ID.

---

## 3. `predicted_locations`

> 실내 위치 예측 모델이 반환하는 각 위치 ID에 대한 상세 정보를 저장하는 마스터 테이블입니다.

-   **`id`** (`VARCHAR(50)`, **PK**, `NOT NULL`)
    -   위치의 고유 ID (모델의 예측 결과와 일치하는 문자열).
-   **`location_name`** (`VARCHAR(255)`, **UNIQUE**)
    -   위치의 이름 (예: "3층 로비", "서편 휴게실").
-   **`description`** (`TEXT`)
    -   위치에 대한 상세 설명.
-   **`floor`** (`INTEGER`, `NOT NULL`, `DEFAULT 3`)
    -   해당 위치의 층수.
-   **`address`** (`VARCHAR(255)`, `NOT NULL`)
    -   건물 주소 (예: "영남대학교 IT관").

---

## 4. `outdoor_place`

> 외부 장소에 대한 지리 정보 및 상세 정보를 저장합니다.

-   **`place_id`** (`VARCHAR(50)`, **PK**)
    -   장소의 고유 ID.
-   **`name`** (`VARCHAR(200)`, `NOT NULL`)
    -   장소의 이름.
-   **`category`** (`VARCHAR(100)`)
    -   장소의 카테고리 (예: "공원", "카페", "관광지").
-   **`address`** (`VARCHAR(255)`)
    -   장소의 주소.
-   **`lon`** (`DOUBLE`, `NOT NULL`)
    -   경도 (Longitude, X 좌표).
-   **`lat`** (`DOUBLE`, `NOT NULL`)
    -   위도 (Latitude, Y 좌표).
-   **`has_facility`** (`BOOLEAN`, `DEFAULT FALSE`)
    -   주요 편의시설의 존재 여부.
-   **`facility_name`** (`VARCHAR(150)`)
    -   편의시설의 이름 (예: "수성못 관광안내소").
-   **`facility_type`** (`VARCHAR(100)`)
    -   편의시설의 종류 (예: "화장실", "안내소").
-   **`facility_location`** (`VARCHAR(255)`)
    -   편의시설의 상세 위치 설명.
-   **`is_accessible`** (`BOOLEAN`, `DEFAULT FALSE`)
    -   교통약자 접근 가능 여부.




<br>

## 🎬 시연 화면  
앱 실행 화면 및 시연 GIF는 아래와 같습니다.  

![Demo Screenshot](./images/demo_screenshot.png)  
*Fig.2 앱 실행 화면*  

![Demo GIF](./images/demo_walkthrough.gif)  
*Fig.3 실시간 길안내 시연*  
<br>

## 📱 UI 구성  
<img width="1280" height="720" alt="슬라이드4" src="https://github.com/user-attachments/assets/9052cb10-b1bd-467e-a447-5e1a753293c9" /><br>

## ✨ 기대 효과  
- **추가 인프라 설치 불필요** → 저비용·고효율 실내 내비게이션 구현  
- 병원, 지하철 역사, 공항, 쇼핑몰 등 **대형 복합 공간 적용 가능**  
- 교통약자 이동 편의성 증대 및 **스마트 시티 서비스 연계 가능성 확보**  
