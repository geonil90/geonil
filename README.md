# README
## 개발프레임웍 
- Spring Boot 2.1.8
## 언어
- JAVA
## WAS
- Apache Tomcat/9.0.24
## Build
- Maven
## IDE
- STS4.2.2
## Zip파일 내부 디렉토리 구조 설명
1. exam : 전달받은 과제정보
2. ide : 통합개발환경도구. 현 과제에서는 위에 기술한 대로 STS 툴을 활용
3. java : 과제에 사용된 자바, 현 과제에는 1.8 버전 활용
4. utility : 수행 시 필요한 유틸리티 모음 현과제에서는 curl과 postman만 사용.  
   ex) sqldevelopr 같은 DB 툴이나, putty 같은 터미널 연결도구 등.
5. was : 프로젝트에서 사용할 was를 저장, 해당 폴더에 tomcat 8.5.42 가 들어가 있으나, 예시일 뿐 현 과제에서는 임베디드된 tomcat 활용
6. workspace : 개발된 소스 모음.

## 서버 기동 방법
### 1. STS 툴 활용
1. 폴더안의 STS 바로가기 실행
2. Boot Dashboard 탭의 exam 프로젝트 선택 후 서버 Start
### 2. jar 파일 실행
1. cmd창 실행 후 빌드해놓은 jar 파일 경로로 이동 
  ```
  cd (zip압축해제경로)\kakaopay_exam\workspace\exam\target\
  ex) 만약 C 에 압축해제 한 경우 -> cd C:\kakaopay_exam\workspace\exam\target\
  ```
2. jar파일 실행
  ```
  -java -jar exam-0.1.jar
  ```
## API 명세
  1. 2018년, 2019년 각 연도별 합계 금액이 가장 많은 고객을 추출하는 API 개발.(단, 취소여부가 ‘Y’ 거래는 취소된 거래임, 합계 금액은 거래금액에서 수수료를 차감한 금액임)
```
- Request
    - method: GET
    - URI
        - localhost:8084/exam1
- Response
    - Status: 200 OK
    - Body
        - List
            - year: 년도
            - name: 계좌명
            - acctNo : 계좌번호
            - sumAmt: 합계금액          
- 출력결과
[
    {
        "year": "2018",
        "name": "테드",
        "acctNo": "11111114",
        "sumAmt": 28992000
    },
    {
        "year": "2019",
        "name": "에이스",
        "acctNo": "11111112",
        "sumAmt": 40998400
    }
]
```


  2.	2018년 또는 2019년에 거래가 없는 고객을 추출하는 API 개발.
(취소여부가 ‘Y’ 거래는 취소된 거래임)

```
- Request
    - method: GET
    - URI
        - localhost:8084/exam2
- Response
    - Status: 200 OK
    - Body
        - List
            - year: 년도
            - name: 계좌명
            - acctNo : 계좌번호
                  
- 출력결과 (해당 년도의 거래가 모두 취소된 경우 거래가 없다고 판단 리스트에 추가함)
[
    {
        "year": "2018",
        "name": "사라",
        "acctNo": "11111115"
    },
    {
        "year": "2018",
        "name": "제임스",
        "acctNo": "11111118"
    },
    {
        "year": "2019",
        "name": "테드",
        "acctNo": "11111114"
    },
    {
        "year": "2019",
        "name": "제임스",
        "acctNo": "11111118"
    }
]
```

  3.	연도별 관리점별 거래금액 합계를 구하고 합계금액이 큰 순서로 출력하는 API 개발.
 ( 취소여부가 ‘Y’ 거래는 취소된 거래임)

```
- Request
    - method: GET
    - URI
        - localhost:8084/exam3
- Response
    - Status: 200 OK
    - Body
        - List
            - year: 년도
            - dataList: 관리점 별 거래금액 합계 리스트
                - List
                  - brName : 관리점명
                  - brCode : 관리점코드
                  - sumAmt : 합계금액
                  
- 출력결과
[
    {
        "year": "2018",
        "dataList": [
            {
                "brName": "판교점",
                "brCode": "A",
                "sumAmt": 20505700
            },
            {
                "brName": "분당점",
                "brCode": "B",
                "sumAmt": 38484000
            },
            {
                "brName": "강남점",
                "brCode": "C",
                "sumAmt": 20232867
            },
            {
                "brName": "잠실점",
                "brCode": "D",
                "sumAmt": 14000000
            }
        ]
    },
    {
        "year": "2019",
        "dataList": [
            {
                "brName": "판교점",
                "brCode": "A",
                "sumAmt": 66795100
            },
            {
                "brName": "분당점",
                "brCode": "B",
                "sumAmt": 45396700
            },
            {
                "brName": "강남점",
                "brCode": "C",
                "sumAmt": 19500000
            },
            {
                "brName": "잠실점",
                "brCode": "D",
                "sumAmt": 6000000
            }
        ]
    }
]
```

  4.	분당점과 판교점을 통폐합하여 판교점으로 관리점 이관을 하였습니다. 지점명을 입력하면 해당지점의 거래금액 합계를 출력하는 API 개발( 취소여부가 ‘Y’ 거래는 취소된 거래임,)

```
- Request
    - method: POST
    - URI
        - localhost:8084/exam4
    - Body
        - brName : 관리점명

- Response (정상 조회 시)
    - Status: 200 OK
    - Body
        - brName : 관리점명
        - brCode : 관리점코드
        - sumAmt : 합계금액

- Response (분당점 포함 없는 부점 입력시)
    - Status: 404 NOT FOUND
    - Body
        - code : 에러코드
        - 메세지 : 메세지
        
- 판교점 입력시
- 입력 값 
{
"brName":"분당점"
}
- 출력결과
{
    "brName": "판교점",
    "brCode": "A",
    "sumAmt": 171181500
}

- 분당점 입력시
- 입력 값 
{
"brName":"분당점"
}
- 출력결과
{
    "code": "404",
    "메세지": "br code not found error"
}

- 잠실점 입력시
- 입력 값 
{
"brName":"잠실점"
}
- 출력결과
{
    "brName": "잠실점",
    "brCode": "D",
    "sumAmt": 20000000
}

```

## 프로그램 실행 방법
### 1. curl 활용
  1. cmd창 실행 후 curl 설치 경로로 이동 
  ```
  cd (zip압축해제경로)\kakaopay_exam\utility\curl_7.66\bin
  ex) 만약 C 에 압축해제 한 경우 -> cd C:\kakaopay_exam\utility\curl_7.66\bin
  ```
  2. 각 API 수행
  ```
    1. API 1번 실행
      curl -X GET http://localhost:8084/exam1
    2. API 2번 실행
      curl -X GET http://localhost:8084/exam2
    3. API 3번 실행
      curl -X GET http://localhost:8084/exam3
    4. API 4번 '판교점'을 입력값으로 실행 (curl을 통해 json 값을 파라미터로 던질때 한글의 경우 정상적으로 전달이 안되는 문제가 있어서 json 파일로 던짐)
      curl -X POST -H "Content-Type: application/json" -d @paramData1.txt http://localhost:8084/exam4       
    5. API 4번 '분당점'을 입력값으로 실행
      curl -X POST -H "Content-Type: application/json" -d @paramData2.txt http://localhost:8084/exam4   
    6. API 4번 '잠실점'을 입력값으로 실행
      curl -X POST -H "Content-Type: application/json" -d @paramData3.txt http://localhost:8084/exam4   
  ```
### 2. postman 활용
  1. 아래 위치의 postman 설치 후 실행
  ```
  (zip압축해제경로)\kakaopay_exam\utility\Postman-win64-7.8.0-Setup.exe
  ```
  2. 위 API 명세에 맞게 수행
  
### 3. index.html 페이지에서 직접 수행

## 문제 해결 방법
- ㅁㄴㅇㄹ
