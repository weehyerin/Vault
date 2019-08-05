## Dynamic Secrets


액세스 할 때 동적으로 비밀 정보가 생성될 때. 
동적인 비밀은 읽을 때까지 존재하지 않으므로 다른 사람이 같은 비밀을 사용하여 훔칠 위험이 없다. 

**Vault에는 해지 메커니즘이 내장되어 있어, dynamic secret은 사용 직후 해지되어 비밀이 존재하는 시간을 최소화 할 수 있다.**


## AWS dynamic secret engine 사용하기

 AWS 비밀 엔진은 사용하기 전에 활성화 해야함. 이 단계는 일반적으로 구성 관리 시스템을 통해 수행됩니다.

~~~
vault secrets enable -path=aws aws
~~~

결과 : 
<img width="554" alt="스크린샷 2019-08-05 오후 5 22 37" src="https://user-images.githubusercontent.com/37536415/62449652-a4918d80-b7a5-11e9-869a-d677faf56895.png">

## AWS dynamic secret engine 구성하기
- **AWS를 인증하고 통신하도록 엔진을 구성**

1. 루트 계정 키를 사용하기
~~~
vault write aws/config/root \
    access_key=AKIAI4SGLQPBX6CSENIQ \
    secret_key=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 \
    region=us-east-1
~~~

### access_key && secret_key 생성하기

1. aws 접속

2. 내 보안 자격 증명
<img width="1123" alt="스크린샷 2019-08-05 오후 5 36 01" src="https://user-images.githubusercontent.com/37536415/62450570-8a58af00-b7a7-11e9-9998-d8042cb4c0bc.png">

3. 왼편에서 사용자 선택
<img width="1119" alt="스크린샷 2019-08-05 오후 5 36 44" src="https://user-images.githubusercontent.com/37536415/62450637-abb99b00-b7a7-11e9-909a-b1607d1f8f4d.png">

4. 사용자 추가

<img width="1123" alt="스크린샷 2019-08-05 오후 5 38 16" src="https://user-images.githubusercontent.com/37536415/62450813-ffc47f80-b7a7-11e9-8e1a-b8c6795917ef.png">
<img width="1119" alt="스크린샷 2019-08-05 오후 5 38 25" src="https://user-images.githubusercontent.com/37536415/62450779-f0ddcd00-b7a7-11e9-8e69-92be8ffdc0ac.png">
<img width="1118" alt="스크린샷 2019-08-05 오후 5 38 46" src="https://user-images.githubusercontent.com/37536415/62450783-f20efa00-b7a7-11e9-8831-3fc42a793a59.png">



********


## Create a Role

Vault에서 액세스 키를 생성하면이 정책이 자동으로 첨부됩니다.
생성 된 액세스 키는 IAM 또는 다른 AWS 서비스가 아닌 EC2에 대한 전체 액세스 권한을 갖습니다. 

**aws / roles / : name이라는 특수 경로를 사용하여 Vault에 IAM 정책을 작성**

1. 
~~~
vault write aws/roles/my-role credential_type=iam_user policy_document=-<<EOF
~~~

2. 
~~~
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "Stmt1426528957000",
      "Effect": "Allow",
      "Action": [
        "ec2:*"
      ],
      "Resource": [
        "*"
      ]
    }
  ]
}
~~~

3. 
~~~
EOF
~~~

<img width="569" alt="스크린샷 2019-08-05 오후 5 43 24" src="https://user-images.githubusercontent.com/37536415/62451152-a6108500-b7a8-11e9-93c6-5df5874f3b17.png">



******


## secret 생성하기 

이제 AWS 비밀 엔진이 활성화되고 역할이 구성되었으므로
aws / creds / : name에서 읽음으로써 Vault에 해당 역할에 대한 액세스 키 쌍을 생성하도록 요청할 수 있다.








