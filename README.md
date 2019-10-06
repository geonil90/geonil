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
                "brName": "분당점",
                "brCode": "B",
                "sumAmt": 38484000
            },
            {
                "brName": "판교점",
                "brCode": "A",
                "sumAmt": 20505700
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

## 문제 해결 방법
- 과제를 받았을 때 처음든 생각이 'DB를 구성해서 해당 내용을 SQL을 통해 조회하면 쉽게 끝나겠다.'였습니다. 그래서 처음에는 spring Boot에 내장되어있는 H2 DB를 활용해서 제공받은 CSV 파일을 읽어 DB에 저장하는 기능을 구현하고 서비스단 로직은 전부 SQL을 통해 조회하는 형태로 구현하려고 했습니다만, 그렇게 되면 너무 쉽게 끝나는 것 같아서 DB구성 없이 CSV 파일을 읽어 java 단에서 데이터를 가공하고 리턴하는 API를 구현하기로 하였습니다.
   1. API 1번 해결과정
   ```
    - 해당 서비스는 2018, 2019년 각 연도별 합계 금액이 가장 많은 고객을 추출하는 API로 문제에서는 2018,2019년 이지만, 추후 다른 연도에 대한 결과도 조회할 수 있어야 하므로, 향 후 서비스 로직의 수정을 최소화 하기 위해 연도를 입력받고 해당 연도 별 합계금액이 가장 많은 고객을 추출하는 형태의 서비스를 개발하였습니다.
      1. 해당 서비스에 필요한 CSV 파일을 읽는다. (거래내역, 계좌정보)
      2. 거래내역 리스트에서 취소된 내역을 제외하고 해당년도의 계좌별 거래금액을 합산하여 Map에 저장한다.
      3. 저장된 맵 중에서 년도별 거래금액의 최대값과 그 계좌번호를 구한다.
      4. 리턴 값에 맞게 리턴한다.
   ```
   2. API 2번 해결과정
   ```
    - 해당 서비스는 2018년 또는 2019년에 거래가 없는 고객을 추출하는 API로 문제에서는 2018년 또는 2019년 이지만, 향후에는 다양한 연도에서 거래가 없는 고객을 추출할 수 있어야 하므로, 향 후 서비스 로직의 수정을 최소화 하도록 연도를 입력받아 해당연도의 거래가 없는 고객의 리스트를 리턴하는 서비스를 개발하였습니다. 
      1. 해당 서비스에 필요한 CSV 파일을 읽는다. (거래내역, 계좌정보)
      2. 거래내역 리스트에서 취소된 내역을 제외하고 해당년도의 거래내역만을 뽑아내서 별도의 해쉬Set에 저장한다.
      3. CSV 파일에서 읽어온 계좌정보중에 2번에 저장한 HashSet에 없는 항목만 리턴 값에 세팅 후 리턴한다.
   ```
   3. API 3번 해결과정
   ```
    - 해당 서비스는 연도별 관리점별 거래금액 합계를 구하고 합계금액이 큰 순서로 출력하는 API로 1,2번과 달리 고려해야 할 사항이 sort였습니다. sql을 이용하면, order by를 활용하면 되지만, sql을 사용하지 않기 때문에 리턴하는 dataList의 VO에 Comparable 인터페이스를 구현하고 Collections.sort를 활용하여 정렬하도록 구현하였습니다.
      1. 해당 서비스에 필요한 CSV 파일을 읽는다. (거래내역, 계좌정보, 관리점정보)
      2. 거래내역 리스트에서 취소된 내역을 제외하고 해당년도의 계좌별 거래금액을 합산하여 Map에 저장한다.
      3. 맵에 저장된 해당년도 계좌별 합계 금액에서 관리점 별 합산금액을 계산하여 Map에 저장한다.
      4. 리턴되는 포맷에 맞게 해당 연도별 관리점별 합산금액을 VoList에 세팅
      5. 합산금액이 큰 순서로 정렬하기 위해 Collections.sort를 활용하여 내림차순으로 정렬
      6. 최종 리턴
   ```
   4. API 4번 해결과정
   ```
    - 해당 서비스는 지점명을 입력하면 해당지점의 거래금액 합계를 출력하는 API로 위의 API와는 다르게 입력값이 있었습니다. 그리고 통폐합한 분당점을 입력하거나, 없는 부점을 입력하면, 404 NOT FOUND를 리턴해야 하므로, BrCodeNotFoundException을 별도로 정의하고 ControllerAdvice를 활용하여 해당 예외 핸들러를 정의하여 처리하였습니다.
      1. 해당 서비스에 필요한 CSV 파일을 읽는다. (거래내역, 계좌정보, 관리점정보)
      2. 입력받은 부점이 관리점정보에 없는 부점이거나, 통폐합한 '분당점'을 입력한 경우 새로 정의한 BrCodeNotFoundException으로 throw 처리한다.
      3. 거래내역 리스트에서 취소된 내역을 제외하고 계좌별 거래금액을 합산하여 Map에 저장한다.
      4. 맵에 저장된 해당년도 계좌별 합계 금액에서 관리점 별 합산금액을 계산하여 Map에 저장한다.
      5. 리턴되는 포맷에 맞게 관리점별 거래합산금액을 세팅한다. 단, 판교점의 경우, 분당점의 거래금액도 합산한다.
      6. 최종 리턴
   ```
