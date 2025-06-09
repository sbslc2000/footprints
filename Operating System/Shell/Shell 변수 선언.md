---
상위 개념: "[[Shell]]"
---
# Shell 변수 선언
Shell에서는 로컬변수, 전역변수, 환경변수, 예약변수, 매개변수 등을 구성할 수 있다.

## 일반적인 특징
* 변수는 대, 소문자를 구별한다.
* 변수의 이름은 숫자를 포함할 수 있지만, 숫자로 시작할 수는 없다.
* 모든 값은 문자열로 저장된다.
* 값을 사용할 때에는 변수명 앞에 $를 사용한다.
* 값을 대입할 때에는 $을 사용하지 않는다.
* 변수를 생성할 때에는 '=' 대입문자 앞뒤로 공백이 없어야 한다.

```bash
#!/usr/bin/bash
name=pete

echo "hello my name is ${name}"
```

## 전역변수와 지역변수
쉘에서 선언된 변수는 기본적으로 전역변수이며, 함수 안에서 local 예약어를 붙인다면 지역변수로 사용할 수 있다.
```bash
#!/usr/bin/bash

name=pete

function print_my_name() {
        local name=john
        echo ${name}
}

echo "hello my name is ${name}"
print_my_name

```

```shell
bash test.sh
hello my name is pete
john
```

## 매개 변수
스크립트를 실행하는 시점에 매개변수를 제공할 수 있다.
```shell
./anyScript p1 p2 p3 ... pn
```

이 때 pn 은 $n에 매핑된다.
```bash
#!/usr/bin/bash
echo $1 $2 $3
```
```shell
bash test.sh Hello World !
Hello World !
```

## 예약 변수
쉘스크립트에서는 특수목적으로 사용되는 예약어가 존재한다.

| 변수명      | 설명                |
| -------- | ----------------- |
| HOME     | 사용자의 홈 디렉토리       |
| PATH     | 실행 파일의 경로         |
| UID      | 사용자의 UID          |
| SHELL    | 사용자가 로그인 시 실행되는 쉘 |
| USER     | 사용자 계정 이름         |
| FUNCNAME | 현재 실행중인 함수 이름     |
```bash
#!/usr/bin/bash

echo $HOME
echo $PATH
echo $LANG
echo $UID
echo $SHELL
echo $USER
echo $FUNCNAME
echo $TERM
```

```shell
bash test.sh
/Users/seobeomseok
/Users/seobeomseok/.nvm/versions/node/v20.15.0/bin:/opt/homebrew/sbin:/opt/homebrew/bin:/opt/homebrew/bin:/Users/seobeomseok/dev/google-cloud-sdk/bin:/opt/homebrew/bin:/usr/local/bin:/System/Cryptexes/App/usr/bin:/usr/bin:/bin:/usr/sbin:/sbin:/var/run/com.apple.security.cryptexd/codex.system/bootstrap/usr/local/bin:/var/run/com.apple.security.cryptexd/codex.system/bootstrap/usr/bin:/var/run/com.apple.security.cryptexd/codex.system/bootstrap/usr/appleinternal/bin:/Applications/Wireshark.app/Contents/MacOS:/Users/seobeomseok/.cargo/bin:/Applications/iTerm.app/Contents/Resources/utilities:/Users/seobeomseok/.local/bin:/Users/seobeomseok/Library/Application Support/JetBrains/Toolbox/scripts
ko_KR.UTF-8
501
/bin/zsh
seobeomseok

xterm-256color
```