## Start - CLI를 사용하여 임의의 비밀 정보를 안전하게 읽고 쓰기

**Vault의 핵심 기능 중 하나는 임의의 비밀 정보를 안전하게 읽고 쓸 수 있다는 것.**

*****

Vault에 기록 된 비밀 키는 암호화 된 다음 백엔드 저장소에 기록
dev 서버의 경우, 백엔드 스토리지는 메모리에 있지만 프로덕션 환경에서는 디스크에 있음.
저장소는 저장 장치 드라이버에 전달되기 전에 값을 암호화 한다. 
백엔드 저장 메커니즘은 Vault로 암호를 해독해야만 한다.

*******

## 예제

- foo = world 쌍을 secret / hello 경로에 쓰기. 
- 경로 앞에 secret /가 붙는 것이 중요. secret / : 임의의 secret을 읽고 쓸 수있는 곳.

client 쪽에서 작성
~~~
vault kv put secret/hello foo=world
~~~

**결과** 
: 
<img width="329" alt="스크린샷 2019-08-05 오후 4 33 27" src="https://user-images.githubusercontent.com/37536415/62446839-c63b4680-b79e-11e9-81fd-99cdabfad0c7.png">


여러 개의 데이터도 쓸 수 있음

~~~
vault kv put secret/hello foo=world excited=yes
~~~~

**명령어**
~~~
vault kv put
~~~

- 명령 행에서 직접 데이터를 쓰는 것 외에도 STDIN과 파일에서 값과 키 쌍을 읽을 수 있다. 
- command 명령어에 대한 설명 : https://www.vaultproject.io/docs/commands/index.html



******

 ## 위에서 저장한 값 가져오기
 
 ~~~
 vault kv get secret/hello
 ~~~
<img width="455" alt="스크린샷 2019-08-05 오후 4 37 40" src="https://user-images.githubusercontent.com/37536415/62447044-6002f380-b79f-11e9-9bef-4e87e59e65c4.png">

>> **Vault는 저장소에서 데이터를 가져 와서 해독**
