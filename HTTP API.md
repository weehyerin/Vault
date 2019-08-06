Vault의 모든 기능은 CLI 외에 HTTP API를 통해 액세스할 수 있다. 사실, CLI로부터의 대부분의 통화는 실제로 HTTP API를 호출한다. 

Vault 기능은 CLI를 통해 사용할 수 없고 HTTP API를 통해서만 액세스 할 수 있는 경우도 있다.

Vault 서버를 시작하면 curl 또는 다른 http 클라이언트를 사용하여 API 호출을 할 수 있다. 예를 들어, Vault 서버를 개발 모드에서 시작한 경우 다음과 같이 초기화 상태를 확인할 수 있다.

~~~
curl http://127.0.0.1:8200/v1/sys/init
~~~
<img width="563" alt="스크린샷 2019-08-06 오후 3 44 14" src="https://user-images.githubusercontent.com/37536415/62516840-0fec6580-b861-11e9-9097-3bdc7546f408.png">

## REST API를 통한 보안 액세스

REST API를 통해 Vault에 액세스할 때. (예를 들어 시스템에서 AppRole을 인증에 사용하는 경우 응용 프로그램은 먼저 Vault API 토큰을 반환하는 Vault에 대해 인증)

이 가이드의 목적상, TLS를 비활성화하고 파일 기반 백엔드를 사용하는 다음 구성을 사용할 것이다. 여기서 TLS는 예시 목적으로만 비활성화된다. TLS는 생산 시 절대로 비활성화되어서는 안 된다.



1. `vi config.hcl`.  명령어. 내부의 기존 값들 지우고 아래 명령어 실행

~~~
backend "file" {
  path = "vault"
}

listener "tcp" {
  tls_disable = 1
}
~~~

2. 
~~~
vault server -config=config.hcl
~~~

`Error initializing listener of type tcp: listen tcp 127.0.0.1:8200: bind: address already in use` <br> 이런 에러가 뜬다면, 기존 서버으 실행을 중지

결과 : <img width="567" alt="스크린샷 2019-08-06 오후 3 49 26" src="https://user-images.githubusercontent.com/37536415/62517186-ca7c6800-b861-11e9-946a-a39ae2d62de2.png">


3. 이제 우리는 우리의 모든 상호작용에 Vault의 API를 사용할 수 있다. 예를 들어 다음과 같이 Vault를 초기화할 수 있다.
> <다른 terminal 창>
~~~
curl \
    --request POST \
    --data '{"secret_shares": 1, "secret_threshold": 1}' \
    http://127.0.0.1:8200/v1/sys/init | jq
~~~

- mac에 jq가 설치 되어있지 않아서 
<img width="565" alt="스크린샷 2019-08-06 오후 3 51 53" src="https://user-images.githubusercontent.com/37536415/62517412-542c3580-b862-11e9-8e80-42f01d1fd9da.png">

아래와 같은 에러가 발생했다. 

`brew install jq` 한 후 다시 시행해보니 아래와 같은 사진이 나왔다. 

<img width="569" alt="스크린샷 2019-08-06 오후 3 53 09" src="https://user-images.githubusercontent.com/37536415/62517414-542c3580-b862-11e9-9119-d16be8696e0c.png">

아마 위의 과정에서 이미 init이 시행된 것 같다. root token을 알아내야 하므로 다시 config.hcl 파일부터 시행했다. 

~~~
backend "file" {
  path = "vault"
}

listener "tcp" {
  tls_disable = 1
}
~~~
여기서 `vault`가 아니라 `test`를 입력

이후 다시 상단의 명령어를 입력함.(아래 사진: 결과)

<img width="571" alt="스크린샷 2019-08-06 오후 4 04 04" src="https://user-images.githubusercontent.com/37536415/62518209-fc8ec980-b863-11e9-9f37-3c5347c24128.png">

~~~
 export VAULT_TOKEN=<사진에 나온 root token>
~~~

~~~
curl \
    --request POST \
    --data '{"key": "<keys_base64의 값>"}' \
    http://127.0.0.1:8200/v1/sys/unseal | jq
~~~

결과 : <img width="567" alt="스크린샷 2019-08-06 오후 4 10 11" src="https://user-images.githubusercontent.com/37536415/62518530-c140ca80-b864-11e9-8dbb-479628ce2013.png">


이제 사용 가능한 모든 인증 방법을 활성화하고 구성할 수 있다. 이 가이드의 목적을 위해 AppRole 인증을 사용하도록 설정하십시오.


*****



## AppRole 인증을 사용하도록 설정하기





