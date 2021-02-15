# Dongjakgu_data_analysis
동작구 2020 빅데이터 활용 정책 공모전


# 1. 분석 목적 및 배경

### 1.1. 목표 : 입지 추천

### 1.2. 배경 : 수요에 비해 공급이 적다.

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/bfb54dc2-1a02-4fed-868e-7829d4a165ff/__.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/bfb54dc2-1a02-4fed-868e-7829d4a165ff/__.png)

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/6ad65f1c-93a7-4dc7-9ce7-4d4e260d6001/.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/6ad65f1c-93a7-4dc7-9ce7-4d4e260d6001/.png)

### 수집데이터

- 서울시 고등학교 수

    [서울시 고등학교 수.xlsx](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/d83af9cf-eb1f-4e0d-b0a5-52235d247f6b/_.xlsx)

    - 출처 : [https://kess.kedi.re.kr/post/6684877?itemCode=04&menuId=m_02_04_02](https://kess.kedi.re.kr/post/6684877?itemCode=04&menuId=m_02_04_02)
    - 데이터 기준 : 2019-09-29

- 서울시 청소년 인구 수 통계

    [서울시 청소년 인구 수.xlsx](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/84999cd6-2687-4119-b5f0-d0eabf3211bd/.xlsx)

    - 출처 : [http://data.seoul.go.kr/dataList/10787/S/2/datasetView.do](http://data.seoul.go.kr/dataList/10787/S/2/datasetView.do)
    - 데이터 기준 : 2020-02-26

---

# 2. 분석 순서

### 2.1. 행정동 단위로 클러스터링

### 수집 데이터

- 주거 인구

    [동작구_주민등록인구_통계_v1.csv](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/cc7729e2-9024-4957-81df-52000937e538/___v1.csv)

    - 출처 : [https://data.seoul.go.kr/dataList/10727/S/2/datasetView.do](https://data.seoul.go.kr/dataList/10727/S/2/datasetView.do)
    - 데이터 기준 : 2019-10-28
- 현재 고등학교 갯수

    [동작구_고등학교_통계.csv](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/87b61d58-5a57-4e15-b220-e6d9549405b2/__.csv)

    - 출처 : [https://kess.kedi.re.kr/post/6684877?itemCode=04&menuId=m_02_04_02](https://kess.kedi.re.kr/post/6684877?itemCode=04&menuId=m_02_04_02)
    - 데이터 기준 : 2020-04-21
- 연령별 인구(13-19)

    [동작구_연령별_통계_v1.csv](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/006c53ae-43ac-401d-afb2-b395c53c031d/___v1.csv)

    - 출처: [http://27.101.213.4/#](http://27.101.213.4/#)
    - 데이터 기준 : 2020-03-31

- 데이터 전처리
    1. raw data 수집
    2. 행정동 단위로 aggregation 진행
    3. albow method로 최적의 클러스터 개수 결정

    ![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/97bb78af-f8a5-47aa-bbc7-45bcb1e2bc94/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/97bb78af-f8a5-47aa-bbc7-45bcb1e2bc94/Untitled.png)

    ![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/0bfc013b-afa1-435c-b771-8ae608d65842/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/0bfc013b-afa1-435c-b771-8ae608d65842/Untitled.png)

    4. 4개의 클러스터로 나눈 후 각 클러스터의 특징 파악

    ![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/56ec6cb4-fef9-4896-9f5b-fb0f959e39c7/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/56ec6cb4-fef9-4896-9f5b-fb0f959e39c7/Untitled.png)

    5. 클러스터 특징

    1) 클러스터1 : 대방동,신대방1동

    → 거주인구가 많고 10대 비율이 타 지역보다 많다.
    이 중 대방동은 이미 고등학교가 4개 설립되어 있다.

    2) 클러스터 2 : 사당2동, 사당5동, 신대방2동

    → 2번째로 10대 비율이 많고, 이미 모두 고등학교가 설립된 지역

    3) 클러스터 3 : 노량진1동, 사당3동, 상도1동,상도2동,상도3동, 상도4동,흑석동

    → 고등학교가 설립되어 있지 않으며, 2번째 클러스터와 10대 비율이 비슷함

    4) 클러스터 4 : 노량진2동, 사당1동

    → 세대당 가구원수가 다른 지역에 비해 훨씬 적음 (1인가구 및 고시원 영향인듯.)

    가장 중요한 10대 비율이 낮다.

    6. 클러스터 선정

    → 이미 고등학교가 설립된 지역과 10대 비율이 낮은 지역은 필터에서 제거하여

    클러스터1과 4에서 선정하되 이미 고등학교가 많이 설립된 대방동은 제거.

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/fae3fa41-1502-4453-8f6b-83e4ae7c0c2b/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/fae3fa41-1502-4453-8f6b-83e4ae7c0c2b/Untitled.png)

→ 신대방1동,상도3동,상도4동,상도2동,노량진1동,상도1동,흑석동,사당3동 8개 필터링

---

### 2.2. 행정동 2차 필터링

### 수집데이터

- 치안등급
    - 출처 : [https://www.safemap.go.kr/main/smap.do?coreThemaMap=&flag=2&level=&searchOpt=&searchTxt=&sideMenu=3](https://www.safemap.go.kr/main/smap.do?coreThemaMap=&flag=2&level=&searchOpt=&searchTxt=&sideMenu=3)
    - 데이터 기준 : 2019-06
- CCTV 개수

    [서울특별시_동작구_CCTV_주소_fin.csv](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/f526ea05-c2e5-41ef-8d40-8fed219371e7/__CCTV__fin.csv)

    - 출처 : [https://data.seoul.go.kr/dataList/OA-13272/F/1/datasetView.do](https://data.seoul.go.kr/dataList/OA-13272/F/1/datasetView.do)
    - 데이터 기준 : 2020-03-27
- 버스 정류장 개수

    [서울시버스정류소좌표데이터(2020.03.06).xlsx](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/a204ea3d-8686-4103-ab03-56f5d017580f/(2020.03.06).xlsx)

    - 출처 : [https://data.seoul.go.kr/dataList/OA-15067/S/1/datasetView.do](https://data.seoul.go.kr/dataList/OA-15067/S/1/datasetView.do)
    - 데이터 기준 : 2020-03-06

- 데이터 전처리
    1. 각 피처들의 특징을 동일한 scale로 스케일링 진행
        - 버스정류장 개수, cctv개수, 치안 등급을 정량화
    2. 종합 인덱스 지표 생성
        - (버스정류장(교통) x 0.5) + (cctv * 0.25) + (safe * 0.25)
        - 흑석동 > 노량진1동 > 상도1동 > 상도4동 > 상도3동 > 사당3동 > 상도2동 > 신대방1동

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/9756e4bd-2f71-4617-a5c6-dbb45476dbf9/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/9756e4bd-2f71-4617-a5c6-dbb45476dbf9/Untitled.png)

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/d342db82-aeba-4697-b88a-b7e394ef0961/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/d342db82-aeba-4697-b88a-b7e394ef0961/Untitled.png)

→ 이중에서 접근성과 안정성 모두 상위에 스코어링 되어있는 노량진1동, 흑석동 필터링

[행정동별 종합지표](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/9d7bfd50-60c3-42b2-aa39-7efc716065c0/hangdata_v2.csv)

---

### 2.3. 상위 집단 대상으로 필터링 - 최적의 입지

### 수집데이터

- 가용지 구분

    [서울_토지특성정보.csv](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/15c29a30-3aaa-41a9-a605-1f57025be010/.csv)

    - 출처 : [http://openapi.nsdi.go.kr/nsdi/eios/ServiceDetail.do](http://openapi.nsdi.go.kr/nsdi/eios/ServiceDetail.do) → 토지특성정보
    - 데이터 기준 : 2019-10-23

- 주변환경

    [서울_행안부_업종데이터](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/81b26396-d855-4d3b-9bda-65379b8ea668/6110000.zip)

    - 출처 : [http://www.localdata.go.kr/devcenter/dataDown.do?menuNo=20001](http://www.localdata.go.kr/devcenter/dataDown.do?menuNo=20001)
    - 데이터 기준 : (기준일 : 최초 인허가 ~ 2020-3-31)
    - 행안부 업종데이터

    [전국_도시계획통계시설정보.csv](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/f13cca0f-a69c-4a6f-bf74-6bbafab1ab06/_.csv)

    - 출처 : [http://openapi.nsdi.go.kr/nsdi/eios/ServiceDetail.do?svcSe=F&svcId=F106](http://openapi.nsdi.go.kr/nsdi/eios/ServiceDetail.do?svcSe=F&svcId=F106)
    - 데이터 기준 : 2020-03-31
    - 국가공간정보포털

# 4. 입지 후보

- 최종적인 아웃풋은 대략적인 '도로명 주소' 나 '번지'로 진행한다.

---

### 참고 자료

- 행정동 위치 좌표 (지도 매핑용)

    [행정_법정동 중심좌표.xlsx](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/9cd85c8f-bbb0-4dc4-b7b3-d2e356aa3267/__.xlsx)

5. 한계점
