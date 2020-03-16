# Crawling_project_kyobo

### Create
- made by : Jungyoung Bae, Bitna Bae
- function : 
		crawling kyobo page
		Save data to database
		sending to slack msg
- return : book_info & review_info


## 데이터 수집의 개요
### 수집 동기
- 책을 접하는 사람들에게 필요한 통합 정보 시스템 필요
    - 책을 만드는 사람 : 베스트셀러 분석을 통한 컨텐츠에 따른 부가기호 발급, 책 사이즈와 같은 제작 트렌드 파악
    - 책을 쓰는 사람 : 자신의 컨텐츠가 속하는 분야, 사이즈, 페이지를 파악하고 각 분야별 베스트셀러를 많이 내놓은 출판사에 대한 정보 얻음
    - 책을 읽는 사람 : 자신이 읽고 싶은 컨텐츠/키워드가 속하는 분야 파악 및 키워드가 포함된 해당 분야 인기 도서 추천받음

- 교보문고 사이트의 특성
    - 가장 유명한 도서 유통 사이트
    - 분야별, 키워드별, 리뷰 정리 등 다양한 독립변수가 잘 정리되어 있음

## 데이터 수집의 계획 및 주기 작성
### 시스템의 구조도
- 약 20개가 넘는 도서 분야 정리(어려운 점: URL 패턴이 복잡, 숫자+영어)
- 각 분야 탭으로 들어가면 베스트셀러 탭을 선택할 수 있음
- 베스트셀러의 경우 8개가 넘는 페이지에 그 주 해당 분야 순위대로 배열
- 구조도
    - 도서 분야별 탭(약 20개 이상)
    - 베스트셀러
        - (한 페이지에 20권씩 총 8페이지 150권 크롤링 가능)
        - 순위별 도서(제목/작가/출판사/가격/출판날짜/평점/책소개요약)
    - 해당도서페이지(앞 페이지 외 키워드/책사이즈/ISBN/책소개/리뷰페이지)
    - 리뷰페이지(리뷰개수 및 리뷰 자체 크롤링 가능)

### 프로세스
#### 크롤링
- 해당 주 분야별 베스트 셀러 TOP 50 크롤링
- 1~50위의 분야/제목/작가/출판사/가격/출판년월일/ISBN/책사이즈(가로*세로)/무게/페이지/책소개요약/키워드/책소개/평점/링크 크롤링
- 리뷰 페이지 크롤링(개수/텍스트)

#### 스크래화피 & MongoDB 저장
- book_info.csv로 저장
- review_info.csv로 저장

#### Merge book_info + review_info dataframe
- 리뷰는 ISBN 기준으로 "".join 으로 한 row에 넣는 코드 작성하여 불러들임

#### 추천 시스템 결과 도출
- 책 만드는 사람 : 문장을 입력하면 가장 일치하는 분야 및 도서 정보 도출(일치할 확률 점검) : 사이즈/페이지는 평균값 정도
- 책 쓰는 사람 : 문장을 입력하면 가장 일치하는 분야 사이즈/페이지/해당 분야 TOP 5 출판사 도출
- 책 읽는 사람 : 읽고 싶은 문장을 쓰면 가장 일치하는 분야/도서 정보(가격/책소개요약/링크)

## 이슈 레벨 정리
- eqalization of term : level 1(30 min)
- category pattern analyze : level 2(1 hour)
- detail page elemention postion analyze : level 2(1 hour)
- review page url finding : level 2(1 hour)
- null exception : level 2(1 hour)

### 버전 히스토리
- 0.1 crawling code
- 0.2 detail page Scrapy
- 0.3 reveiw page Scrapy
- 0.4 detail page Slack MSG, book in stock
