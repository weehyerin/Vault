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



1. vi config.hcl 명령어. 내부의 기존 값들 지우고 아래 명령어 실행

backend "file" {
  path = "vault"
}

listener "tcp" {
  tls_disable = 1
}
















