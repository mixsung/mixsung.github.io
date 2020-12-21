---
title: "맥북에서 python 버전 업그레이드 하기"
excerpt: "맥북은 python 최신 버전이 자동으로 깔려있습니다. 2.7버전이 깔려있어 최신 버전 3.9로 업그레이드하기 위해 이것저것 만져보았습니다."

categories:
tags: python3 맥북 파이썬 업그레이드

---

맥북은 python이 자동으로 깔려있습니다.
2.7버전이 깔려있어 최신 버전 3.9로 업그레이드하기 위해 이것저것 만져보았습니다. 

## Homebrew 설치
맥과 리눅스를 위한 패키지 관리자입니다.
터미널을 열어 아래의 코드를 붙여넣어 실행합니다.
```
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```
Press RETURN ~ 메세지가 나오면 엔터를 누르고 기다리면 됩니다.   

```
brew help
```
help를 입력해서 명령방법이 출력되면 설치 완료!

Homebrew 공식 사이트: <https://brew.sh/index_ko>

## python3 설치
터미널을 열고 homebrew로 파이썬 설치
```
brew install python3
```
설치됐다는 문구가 뜨고, 확인을 위해 python을 입력해보면 원래 버전 2.7이 남아있는 경우가 있습니다.

그럴 때는 환경변수 파일을 직접 수정하면 해결이 되는데요, 
```
vi ~/.profile
```
터미널에서 위 코드로 환경변수 파일을 열어줍니다.

Shift + d를 누르고 o(or i)를 눌러 아래의 코드를 입력해주면 설정완료
```
alias python='/usr/local/bin/python3'
alias pip='/usr/local/bin/pip3'
```
저장을 위해 esc 누른뒤 :wq!를 입력해주세요

환경변수 파일을 나와서
```
source ~/.profile
```
로 다시 설정해주고 python 입력하면 new version 설치 확인!

<img width="567" alt="스크린샷 2020-12-21 오후 10 36 00" src="https://user-images.githubusercontent.com/26542114/102784907-64ee3480-43e0-11eb-87b2-4ca1412fbd24.png">
