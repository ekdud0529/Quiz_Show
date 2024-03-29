# 유닉스 텀 프로젝트

## 프로젝트 목표
#### 1. 기본적인 O/X 퀴즈 구현
#### 2. 문제 카테고리 생성
- 여러 영역의 문제를 출제하여 client가 원하는 분야의 문제를 풀 수 있게끔 구현할 것이다.
#### 3. 문제 은행 방식
- 텍스트 파일에서 문제 가져오기
- 문제 랜덤하게 출력하기
#### 4. 총 점수 출력
- 총 7문제를 출제하고 마지막에 몇 개의 문제를 맞췄는지 결과를 출력해줄 것이다.
#### 5. 명예의 전당
- 퀴즈 결과로 1~3위를 뽑아서 이름을 올릴 수 있는 명예의 전당이라는 컨셉을 잡았다.
- client의 총 점을 기준으로 이전 도전자들과 비교해, 명예의 전당에 이름을 올렸는지에 대한 결과를 출력해줄 것이다.

## 역할 분담
|김다영|server 구현|
|---|---|
|박예린|client 구현, 문제 textfile 제작|

## 프로토콜
![image](https://user-images.githubusercontent.com/45174177/163175777-5a7dcf72-d644-4352-ae5e-05f00290d18a.png)

## 부가 기능
#### 1. 카테고리
- 카테고리를 만들어 client가 도전하고 싶은 영역의 문제를 고를 수 있도록 구현했다.
- client에게 카테고리를 제시하여 응답을 받는다.
- 응답받은 결과를 가지고 switch 문을 통해 해당 text 파일을 열어 text 파일에 있는 문제를 모두 배열에 넣었다

#### 2. 문제 랜덤 출력
- 각 카테고리마다 총 21개의 문제 중, q[]라는 배열에 문제번호를 겹치는 것이 없도록 무작위로 7개 선정해 넣었다. 따라서 매번 같은 문제가 나올 확률을 줄였다

#### 3. 총 점수 출력
-  client가 문제를 맞혔을 경우를 cnt라는 integer 변수에 저장해 두고 문제를 다 풀었을 경우, 최종 맞춘 문제의 개수를 출력한다.

#### 4. 명예의 전당 진입 결과 출력
![image](https://user-images.githubusercontent.com/45174177/163176342-43ad286c-8fe5-424c-9b53-84c239e99052.png)
- 명예의 전당이라는 컨셉을 잡아 문제 푼 결과가 3등 이내에 진입할 경우 등수를 출력하도록 구현했다.
- rank.txt 파일을 만들어 1~3위인 client의 ID와 총 점을 저장해 두었고, 결과가 바뀔 때마다 갱신할 수 있도록 만들었다.


### sleep()이 자주 등장하는 이유
write와 read의 속도를 맞춰주기 위함이다. client와 server가 서로 read write하는 속도가 맞지 않아 의도치 않은 결과를 
출력하는 것을 방지해주는 역할을 한다.
