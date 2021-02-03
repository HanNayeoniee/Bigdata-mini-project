# Bigdata_mini-project

# 요즘 가장 인기있는 디저트는 무엇일까?

[강의자료](https://nbviewer.jupyter.org/github/smu405/s/tree/master/)

---
## ◼ 1단계: 데이터 수집
 
네이버 api와 크롤링을 통해 네이버 블로그 본문 데이터를 수집
  1. 네이버 api를 사용해 키워드를 "디저트"로 설정한 후 블로그 검색한 결과를 수집
  2. 반환되는 json형식의 데이터에서 블로그 링크 부분만 따로 저장
  3. 총 1100개의 블로그 링크의 본문만 크롤링해 데이터 수집

  코드: [1_naver_api_final.ipynb](https://github.com/HanNayeoniee/Bigdata_mini-project/blob/master/1_naver_api_final.ipynb)
  
  [2_get_crawling_links.ipynb](https://github.com/HanNayeoniee/Bigdata_mini-project/blob/master/2_get_crawling_links.ipynb)
  [3_get_blog_contents.ipynb](https://github.com/HanNayeoniee/Bigdata_mini-project/blob/master/3_get_blog_contents.ipynb)
  
  -네이버 api를 활용하고 크롤링하면 수집할 수 있는 데이터라서 사용한 데이터는 따로 업로드하지 않음
  
  https://github.com/HanNayeoniee/Bigdata_mini-project/blob/master/1)%20naver_api_final.ipynb
  
---
## ◼ 2단계: 데이터 전처리

  1.	정규표현식으로 특수문자, 영어, 숫자는 제거하고 한글만 남기기
    -> 블로그 포스팅 중 이미지를 제외하고 본문의 텍스트만 크롤링했기 때문에 글 사이에 빈 공간 (개행문자)이 많음
    -> 빈 공간을 제거하지 않으면 빈 문자열이나 유니코드가 빈도수가 높은 단어에 속하기 때문에 제거함 
    -> 크롤링한 결과를 rdd로 만든 후 파이썬의 정규표현식 모듈 “re”를 사용함 
    
  2.	stopwords를 처리해 의미 없는 단어와 유니코드 제거
  
  3.	꼬꼬마 형태소 분석기를 사용해 명사만 남기기
    -> tokens 변수 안에 명사만 리스트로 담겨있음

  코드: 4) blog_contents_analysis

---
## ◼ 3단계: 데이터 분석 & 시각화

  1.	디저트 관련 단어만 뽑기 전 분석
    -> 명사만 들어있는 리스트 tokens에서 rdd생성 후 flatMap()을 사용해 단어 빈도 세기
    -> 단어를 빈도수별로 상위 50개의 단어 그래프 그리기
    -> 워드 클라우드로 그리기 (최대 200개의 단어 선택)
    
   2.	디저트 관련 단어만 뽑은 후 분석
    -> 디저트 관련 단어만 뽑지 않으면 “이”, “저”, “그”, “한”, “개”와 같은 지칭명사나 수량명사가 빈도수가 높은 단어에 속하기 때문에 인기있는 디저트를 찾는데 방해가 됨
    -> 디저트 관련 단어 목록(keywords) 만들기: 디저트에 들어가는 과일(청포도, 레몬, 딸기 등), 과일을 제외한 재료(녹차, 초코, 민트, 인절미 등), 종류(빵, 버블티, 아이스크림, 커피, 밀크티 등)관련 단어로 130단어로 구성됨 – “디저트리스트”파일 안에 있음
    -> 명사만 들어있는 리스트 tokens안에 keywords에 해당하는 단어가 있으면 리스트 yes에 append해 디저트 관련 단어만 뽑기
    -> 디저트 관련 단어만 들어있는 리스트 yes에서 rdd생성 후 flatMap()을 사용해 단어 빈도 세기
    -> 단어를 빈도수별로 상위 50개의 단어 그래프 그리기
    -> 워드 클라우드로 그리기 (최대 30개의 단어 선택)

  코드: 4) blog_contents_analysis

---
## ◼ 결론 

  -“요즘 가장 인기있는 디저트는 무엇일까?” 라는 질문에 “커피”라고 대답할 수 있다.
  
  -네이버 블로그 포스팅 1100개의 데이터를 수집해 분석한 결과, 디저트 중에 커피, 마카롱, 티, 빵, 치즈, 아이스크림, 차, 쿠키, 딸기, 타르트 순서로 단어 빈도수가 높았다.
 
