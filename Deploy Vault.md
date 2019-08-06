
지금까지 자동적으로 인증하는 "dev" 서버, 메모리 내 저장장치 설정 등과 함께 작업해 왔다. 
Vault를 실제 환경에 구축하는 방법을 알아볼 예정

******

## Configuring Vault

1. config.hcl 파일 생성
~~~
vi config.hcl
~~~

2. 아래 내용 붙여 넣기
~~~
storage "consul" {
  address = "127.0.0.1:8500"
  path    = "vault/"
}

listener "tcp" {
 address     = "127.0.0.1:8200"
 tls_disable = 1
}
~~~

`storage` - Vault가 저장소에 사용하는 실제 백엔드입니다. 이때까지 dev 서버는 "인메모리"(메모리)를 사용했지만, 백엔드인 Consul을 사용한다.

`listener` - 하나 이상의 수신기가 Vault가 API 요청을 수신하는 방법을 결정한다. 위의 예는 TLS 없이 localhost 포트 8200에서 수신된다. 환경에서 VAULT_ADDR=http://127.0.0.1:8200을 설정하여 TLS 없이 Vault 클라이언트를 연결하기

3. install consul
[install consul](https://learn.hashicorp.com/consul/getting-started/install.html)

위의 url에서 consul을 다운로드 받은 후, vault와 같이 export path 명령어로 경로에 넣어주기



한 개의 terminal 창에서 아래 시행
~~~
consul agent -dev
~~~

다른 터미널에서 아래 시행
~~~
vault server -config=config.hcl
~~~

제대로 됐다면, 

아래와 같은 결과가 나옴. 
1. <br>
<img width="568" alt="스크린샷 2019-08-06 오후 3 21 28" src="https://user-images.githubusercontent.com/37536415/62515666-f0077280-b85d-11e9-93f4-52d316da53ed.png">

2. <br>
<img width="567" alt="스크린샷 2019-08-06 오후 3 21 10" src="https://user-images.githubusercontent.com/37536415/62515665-ef6edc00-b85d-11e9-8b30-bf88cff37846.png">











