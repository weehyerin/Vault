## Policies, 인증 허가

Vault의 policy : 사용자가 액세스할 수 있는 항목을 제어

인증의 경우 Vault에는 실행 및 사용할 수 있는 여러 가지 옵션 또는 방법이 있다. 

모든 인증 방법은 Vault로 구성된 핵심 정책에 ID를 다시 매핑한다.

제거할 수 없는 내장형 정책도 있다. 예를 들어 루트 및 기본 정책은 필수 정책이며 삭제할 수 없다. 

## policy 만들기

1. 
~~~
vi my-policy.hcl
~~~

2.
~~~
path "secret/*" {
  capabilities = ["create"]
}
path "secret/foo" {
  capabilities = ["read"]
}

# Dev servers have version 2 of KV mounted by default, so will need these
# paths:
path "secret/data/*" {
  capabilities = ["create"]
}
path "secret/data/foo" {
  capabilities = ["read"]
}
~~~

my-policy.hcl 여기에 붙여넣기


3. 
~~~
vault policy fmt my-policy.hcl
~~~

결과 : <img width="529" alt="스크린샷 2019-08-06 오후 2 07 14" src="https://user-images.githubusercontent.com/37536415/62512591-82eedf80-b853-11e9-9cb9-d8bb75037d4d.png">


4.  정책을 쓰기 위해, 업로드할 정책 파일의 경로를 지정하기.

~~~
$ vault policy write my-policy my-policy.hcl

Success! Uploaded policy: my-policy
~~~

4-1.
~~~
vault policy write my-policy -<<EOF
~~~

4-2. 
~~~
path "secret/*" {
  capabilities = ["create", "update"]
}
path "secret/foo" {
  capabilities = ["read"]
}

# Dev servers have version 2 of KV mounted by default, so will need these
# paths:
path "secret/data/*" {
  capabilities = ["create", "update"]
}
path "secret/data/foo" {
  capabilities = ["read"]
}
EOF
~~~

결과 : <br>
<img width="551" alt="스크린샷 2019-08-06 오후 2 08 59" src="https://user-images.githubusercontent.com/37536415/62512643-c3e6f400-b853-11e9-81e0-396911b38715.png">


### 결과 확인

~~~
vault policy list
~~~

<img width="410" alt="스크린샷 2019-08-06 오후 2 09 31" src="https://user-images.githubusercontent.com/37536415/62512664-e6790d00-b853-11e9-9d93-53ec46d6f9dc.png">


- 읽기
~~~
vault policy read my-policy
~~~

<img width="520" alt="스크린샷 2019-08-06 오후 2 11 45" src="https://user-images.githubusercontent.com/37536415/62512733-263ff480-b854-11e9-94ca-f4bcbb98bd33.png">



**********


## Testing the Policy

~~~
vault token create -policy=my-policy
~~~

<img width="549" alt="스크린샷 2019-08-06 오후 2 13 03" src="https://user-images.githubusercontent.com/37536415/62512785-630beb80-b854-11e9-8956-2ed457b29bc2.png">

> policy token key value 기억하기 아래에서 login 할 때 사용


~~~
vault login <policy token key value>
~~~

<img width="544" alt="스크린샷 2019-08-06 오후 2 15 10" src="https://user-images.githubusercontent.com/37536415/62512849-aebe9500-b854-11e9-9a7c-61382b6cfda8.png">



**********


~~~
path "secret/*" {
  capabilities = ["create", "update"]
}
path "secret/foo" {
  capabilities = ["read"]
}
~~~

**my-policy 안에 foo는 읽기만 가능하고, 나머지 경로는 create, update가 가능하게 설정해 놓음. 따라서 아래와 같은 결과가 발생함.**


## Dev server(개발자 서버)

~~~
vault kv put secret/bar robot=beepboop
~~~

~~~
vault kv put secret/foo robot=beepboop
~~~

<img width="539" alt="스크린샷 2019-08-06 오후 2 20 30" src="https://user-images.githubusercontent.com/37536415/62513065-78cde080-b855-11e9-9397-0458cb714e30.png">


## Non-dev servers

~~~
vault kv put secret/bar robot=beepboop
~~~

~~~
vault kv put secret/foo robot=beepboop
~~~
<img width="544" alt="스크린샷 2019-08-06 오후 2 23 02" src="https://user-images.githubusercontent.com/37536415/62513136-c34f5d00-b855-11e9-9611-f04523af5ef0.png">

> 정책에 따라 sys에 액세스할 수 없으므로, 볼트 정책 목록 또는 볼트 비밀 목록과 같은 명령이 작동하지 않는다.<br>**계속하려면 초기 루트 토큰으로 다시 인증하기**


## Mapping Policies to Auth Methods

여러 인증 방법을 사용할 수 있는 인증과는 달리 Vault 자체는 단일 정책 권한이다. 
활성화된 인증 방법은 핵심 정책에 ID를 연결해야 한다.

1. github 파일 생성
~~~
vault auth enable -path=github github
~~~

2. github과 my policy 생성
~~~
vault write auth/github/map/teams/default value=my-policy
~~~

<img width="573" alt="스크린샷 2019-08-06 오후 2 40 46" src="https://user-images.githubusercontent.com/37536415/62513802-32c64c00-b858-11e9-9c86-8208e2753f24.png">








