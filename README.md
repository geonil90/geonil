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
4. utility : 수행 시 필요한 유틸리티 모음 현과제에서는 curl만 사용. ex) sqldevelopr 같은 DB 툴이나, putty 같은 터미널 연결도구 등.
5. was : 프로젝트에서 사용할 was를 저장, 해당 폴더에 tomcat 8.5.42 가 들어가 있으나, 예시일 뿐 현 과제에서는 임베디드된 tomcat 활용
6. workspace : 개발된 소스 모음.

## 서버 기동 방법
### 1. STS 툴 활용
1. 폴더안의 STS 바로가기 실행
2. Boot Dashboard 탭의 exam 프로젝트 선택 후 서버 Start
### 2. jar 파일 실행
1. cmd창 실행 후 빌드해놓은 jar 파일 경로로 이동 
  -cd (zip압축해제경로)\kakaopay_exam\workspace\exam\target\
  -ex) 만약 C 에 압축해제 한 경우 -> cd C:\kakaopay_exam\workspace\exam\target\
2. jar파일 실행
  -java -jar exam-0.1.jar

## 프로그램 실행 방법
### 1. curl 활용
### 2. postman 활용
### 3. index.html 페이지에서 직접 수행

## 문제 해결 방법
- ㅁㄴㅇㄹ
