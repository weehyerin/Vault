# Install

## 다운로드 경로
https://www.vaultproject.io/downloads.html

## 경로에 넣어주기
환경 변수 경로에 넣는 법

 vault 파일이 download에 있으므로 
~~~
 export PATH=$PATH:/Users/weehyerin/Downloads/
~~~

원래는 path 경로에 download가 없었는데, 생기는 것을 볼 수 있음

<img width="571" alt="스크린샷 2019-08-05 오후 3 38 59" src="https://user-images.githubusercontent.com/37536415/62443981-452c8100-b797-11e9-924f-a6d4237677e6.png">

**원래 vault bash not found라고 나왔던 것이 vault라는 명령어를 인식하는 것을 알 수 있음
<img width="473" alt="스크린샷 2019-08-05 오후 3 40 50" src="https://user-images.githubusercontent.com/37536415/62444073-7f961e00-b797-11e9-8990-3b00cb300ce0.png">

~~~
vault -autocomplete-install
~~~

********

# starting dev server
**dev 서버는 안전하지 않지만 Vault를 로컬에서 재생할 때 유용하게 사용할 수있는 미리 구성된 기본 제공 서버**

~~~
vault server -dev
~~~

<img width="571" alt="스크린샷 2019-08-05 오후 3 49 51" src="https://user-images.githubusercontent.com/37536415/62444525-af91f100-b798-11e9-96ad-35e1163a9be9.png">


**아래 port로 로컬에서 접속할 수 있음**
http://127.0.0.1:8200

아래 사진과 같은 화면이 뜨면 성공
<img width="1125" alt="스크린샷 2019-08-05 오후 3 50 50" src="https://user-images.githubusercontent.com/37536415/62444580-db14db80-b798-11e9-83e7-dd64cdd771aa.png">

********

**새로운 터미널 창 열어서 시행

## Vault 클라이언트가 dev 서버와 통신하도록 구성
~~~
export VAULT_ADDR='http://127.0.0.1:8200'
~~~

## unseal key를 어딘가에 저장. (지금은 아무데나 저장.)

## 생성 된 루트 토큰 값을 복사하면 VAULT_DEV_ROOT_TOKEN_ID 환경 변수로 설정

~~~
export VAULT_DEV_ROOT_TOKEN_ID="s.XmpNPoi9sRhYtdKHaQhkHP6x<루트 토큰의 위치>"
~~~

unseal key와 ROOT_TOKEN은 아까 서버 실행하면서 나타남

<img width="532" alt="스크린샷 2019-08-05 오후 4 03 41" src="https://user-images.githubusercontent.com/37536415/62445226-b4f03b00-b79a-11e9-99c4-3d1293ebe28e.png">

성공 확인

~~~
vault status
~~~

<img width="400" alt="스크린샷 2019-08-05 오후 4 22 04" src="https://user-images.githubusercontent.com/37536415/62446238-321caf80-b79d-11e9-8eec-cb2c504760e8.png">
