# pose
해당 프로젝트는 **"딥러닝을 통한 인스타그램 해시태그 추천 및 사진 분류 웹사이트 구현"** 으로 Java/spring에 대한 깃헙 주소입니다.

python 딥러닝 구현 코드 깃헙 주소는 해당 주소를 참고해주세요.
 - https://github.com/kh6815/pose_DeepLearning

전체적인 동작 구현 영상은 유튜브를 참고해주세요.
 - https://www.youtube.com/watch?v=wnqB6Vz1_Cg

해당 프로젝트에 관한 논문은 dropbox링크를 참고해주세요
 - https://www.dropbox.com/scl/fi/6z8xqxoywebwzgqv4h0qs/.doc?dl=0&rlkey=kiysayk0spo6w9kqfgu7tmcle

## 작품 요약
최근 인스타그램의 사용자들이 많아지면서 각 개인의 게시물의 수 또한 증가하고 있다. 하지만 인스타그램 내에서는 자신이 올린 사진을 분류해서 볼 수 있는 기능이 없다. 
그래서 게시물이 많을수록 사용자가 보고싶은 사진을 찾기 어렵다. 
이 문제를 해결하기 위해 본 논문에서는 사용자의 게시물을 크롤링을 통해 가져와 사진과 정보를 저장하고 그 정보에 따라 사진들을 분류하여 서버를 통해 웹사이트에 띄워주는 방법을 제안한다. 

사진 분류 기준은 게시물을 올린 날짜와 게시물에 붙은 해시태그이다. 날짜의 경우 게시물의 업로드 날짜를 연,월 별로 분류해 폴더를 만들어 저장한다.
해시태그의 경우 본인이 자주 사용한 해시태그 위주로 폴더를 만들어 분류한다. 해시태그는 의미가 같은 단어의 변형이 많기 때문에 정형화가 필요하다. 
해당 프로젝트에서는 DRN(Dilated Residual Network) 딥러닝 모델을 만들어 사용자가 게시물을 올릴 때 사진을 분석해서 해시태그를 추천하도록 한다. 
DRN 모델은 정확도가 73%로 교육시킨 사진의 양에 비해 높은 적중률을 가지고 있다. 해시태그를 추천할 경우 사용자가 자주 사용하는 해시태그가 정형화 되고 분류가 더욱 정확해진다. 
사진 분류가 정확해질수록 사진 찾기는 쉬워짐을 알 수 있다.

## 기술 스택
 - 구현 언어 : Java, python, HTML/CSS/JS
 - 프레임워크 : Spring Framework 
 - IDE : IntelliJ, Visal Studio Code, Pycharm 
 - 딥러닝 모델 : DRN 모델

## 시스템 설계 및 구현
 ### 해시태그 추천 및 사용자 사진 분류
     - 사용자의 인스타그램 게시글을 작성할 때 사진의 태그를 추천.
     - 사용자의 인스타그램 게시글 데이터를 크롤링하여 날짜와 해시태그별로 분류하여 보여줌.

***
**해시태그 추천 동작 기능**

   ![해시태그 추천 구성도](https://user-images.githubusercontent.com/62634760/130561192-a12f0d0e-3ebd-45d7-9aad-7180940806dc.PNG)

 *사용자가 원하는 이미지를 서버에 업로드 -> 추천 DRN 알고리즘 동작 -> 해당 사진에 적절한 사진 키워드 생성 -> 
 사전에 미리 텍스트 마이닝한 해시태그들과 매칭하여 추천해시태그를 보여줌*

***
**사진 분류 동작 기능**

   ![사진 분류 구성도](https://user-images.githubusercontent.com/62634760/130562151-b6c52897-4dc3-40ca-929f-5d150394e9b6.PNG)
     
 *사용자의 인스타그램 계정을 입력 -> server 측에서 자동적으로 인스타그램에 접속하여 사용자의 게시글을 크롤링 -> 해당 게시글을 날짜 또는 
  해시태그별로 분류하여 보여줌 -> 클릭시 인스타그램의 해당 게시물로 이동*

***
**시스템 흐름도**

   ![시스템 흐름도](https://user-images.githubusercontent.com/62634760/130562874-02302b05-1cb3-4e00-9cd2-5de4194d9b1b.PNG)


  *개인정보동의란에 체크 후 인스타그램 계정으로 pose앱 로그인 ->  해시태그 추천기능과 사진 분류 기능을 실행

  - 해시태그 추천 기능 설명 
     1) 업로드한 사진 파일을 서버 디렉토리에 저장 
     2) Multi Process, 로컬 소켓 서버 통해 해시태그 추천 파이썬 코드를 동작    
     3) DRN 모델과 text mining 데이터를 바탕으로 사진 정보를 판단 
     4) 결과 값 buffer reader를 통해 서버측으로 전달 후 사용자에게 제공

  - 사진 분류 기능 설명  
     1) 사용자 로그인 정보를 서버측으로 전송 후 파이썬 프로세스 실행   
     2) 로컬 소켓 서버를 사용하여 데이터 전송 파이썬 측으로 전달
     3) 사용자의 인스타그램 게시물을 크롤링
     4) Date별 / tag 별 분류 알고리즘  
     5) 분류 별로 사진 데이터를 사용자에게 제공  
     6) 사진을 클릭하면 해당 사진이 포함된 인스타 게시글 이동

***
### 시스템 구현 결과  
  ### 해시태그 추천 동작 결과

  ![해시태그 추천 결과](https://user-images.githubusercontent.com/62634760/130571797-c6a87459-e8fc-47dd-adae-2f852e8e9374.PNG)
   
  사진의 결과 값 = reading
  reading에 해당하는 인스타그램 해시 태그 추천   

  ### 사진 분류 동작 결과
***
   *년/월별 사진 분류*
 
  ![년도별 사진 분류](https://user-images.githubusercontent.com/62634760/130572258-20e0ae69-6940-45d7-be2e-9f212b78f766.PNG)

***
   *태그별 사진 분류*

   ![태그별 사진 분류](https://user-images.githubusercontent.com/62634760/130572334-f86c2f8c-e5e4-4a83-b77d-7f886b5288e0.PNG)  

***
