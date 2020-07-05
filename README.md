# Spring Cloud Server 외부 설정 파일 리포지토리

1\. 설정 방법 
* Spring Cloud Config Server 설정 파일(yml, properties)에서 아래와 같이
해당 설정 파일 리포지토리 uri 및 search-paths 설정.
* Private Repository 경우 username, password 필요.
```$xslt
spring:
  cloud:
    config:
      server:
        git:
          uri: https://github.com/nmrhtn7898/config-repo/
          search-paths: testservice, 디렉토리이름
          username: git 계정 아이디[private repo 경우 필요]
          password: git 게정 비밀번호[private repo 경우 필요]
```
2\. 사용 방법
* [Spring Cloud Config Server](https://github.com/nmrhtn7898/config-server) 엔드 포인트를 통해서 외부 설정 파일 리포지토리 설정 파일을 읽어서 캐시하여 사용.
* 엔드 포인트 url 패턴 */{application-name}/{profile}*
* 사용 예시 /testservice/dev, profile dev 경우 default, dev 설정 파일 모두 사용하고 중복 프로퍼티는 dev 설정 파일에
 존재하는 프로퍼티 값으로 overwrite 한다. 응답 JSON 예시 아래와 같음.
* 암호화 프로퍼티는 {cipher} prefix 적용
```$xslt
{
    "name": "testservice",
    "profiles": [
        "dev"
    ],
    "label": null,
    "version": "23583d91f47e2f7f73ff8c913fd57fdeb20a95d1",
    "state": null,
    "propertySources": [
        {
            "name": "https://github.com/nmrhtn7898/config-repo//testservice/testservice-dev.yml",
            "source": {
                "name.firstname": "sin",
                "name.lastname": "nari",
                "name.password": "{cipher}3376eee3887c9f11c1593536ce075dd1bab11eba5fbdab9f9348da9e06f7c3fa",
                "name.message": "i am dev"
            }
        },
        {
            "name": "https://github.com/nmrhtn7898/config-repo//testservice/testservice.yml",
            "source": {
                "name.firstname": "sin",
                "name.lastname": "youngjin",
                "name.password": "{cipher}9357c645da1f863891a6dbb93399916413841cc086c49d7821f8b7ccd437a192",
                "name.message": "i am default+++++++++++++++++++++++++++++++++++++++"
            }
        }
    ]
}
```