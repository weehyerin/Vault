## 오류 발생

~~~
vault write foo/bar a=b
~~~

결과 : 
<img width="429" alt="스크린샷 2019-08-05 오후 4 53 16" src="https://user-images.githubusercontent.com/37536415/62448010-a35e6180-b7a1-11e9-9214-29367a8a4b23.png">

path prefix는 Vault에게 트래픽을 라우팅해야하는 엔진의 비밀을 알려준다. 

> **요청 Vault에 도달 --> 가장 길게 일치되는 초기 경로 -->  해당 경로에서 활성화 된 해당 비밀 엔진으로 요청을 전달**

>> 기본적으로 Vault는 secret secret / 경로에서 kv라는 비밀 엔진을 사용. 
>> kv secret 엔진은 원시 데이터를 읽고 백엔드 스토리지에 씀


Vault는 kv 외에 다른 많은 비밀 엔진을 지원하며 이 기능은 Vault를 유연하고 독창적으로 만든다. 

**이 페이지는 비밀 엔진과 해당 엔진이 지원하는 작업에 대해 설명.**



******



## secret engine 사용하기

> **kv secret 엔진의 다른 인스턴스를 다른 경로에서 활성화.**

파일 시스템과 마찬가지로 Vault는 다양한 경로에서 비밀 엔진을 사용할 수 있다. 

각 경로는 완전히 격리되어 있으며 다른 경로와 대화 할 수 없다. --> 예를 들어, foo에서 활성화 된 kv 비밀 엔진은 bar에서 활성화 된 kv 비밀 엔진과 통신 할 수 없다.

**비밀 엔진이 사용되는 경로의 기본값은 비밀 엔진의 이름. (아래 두 개의 명령어 중 아무거나 써도 됨)**
~~~
vault secrets enable -path=kv kv

or 

vault secrets enable kv
~~~

<img width="515" alt="스크린샷 2019-08-05 오후 5 00 39" src="https://user-images.githubusercontent.com/37536415/62448408-b6256600-b7a2-11e9-883d-e3e36bae621c.png">

**비밀 엔진이 사용되는 경로의 기본값은 비밀 엔진의 이름. (두 개의 명령어 중 아무거나 써도 됨)**



******



## secret engine 만들기가 잘 되었는 지 확인하기 

~~~
vault secrets list
~~~



