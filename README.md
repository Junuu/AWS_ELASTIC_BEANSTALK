### 아마존 웹서비스
#### Elastic Beanstalk 처음시작부터 배포해보기 ( A to Z)
```
1. Google에 AWS 검색
2. 로그인 후 서비스 -> 컴퓨팅 -> Elastic Beanstalk
3. 새 환경 생성 -> 환경 티어 선택 -> 웹 서버 환경 -> 선택 -> 애플리케이션 이름 설정(Koinvesting) -> 도메인 입력(Koinvesintg) -> 설명 작성(주식 초보자를 위한 기업의 재무제표 분석을 간단하게 정리해주는 사이트입니다.) -> 플랫폼 선택(Python) -> 플랫폼 브랜치(Python 3.7 running on 64bit Amazon Linux2) -> 플랫폼 버전(3.0.3 Recommended) -> 애플리케이션 코드(샘플 애플리케이션) -> 환경생성 -> 몇 분 후 생성완료
4. 왼쪽 navbar 에서 환경으로 이동을 누르면 샘플 사이트로 이동하여 잘 작동하는 것을 볼 수 있습니다.
```
#### 이제 플라스크를 사용하여 웹서버를 구축하여 샘플 사이트가 아닌 제 웹사이트로 바꿔봅시다.
##### [플라스크로 크롤링 웹서버를 만들면서 참고한 레퍼런스들의 모음](https://github.com/Junuu/Tutorial-Flask) python 설치부터 웹서버 코드 작성 방법까지 순차적으로 기재되어 있습니다.
##### [플라스크를 사용하여 자신의 웹사이트 배포](https://www.youtube.com/watch?v=iBeOvmt-tR0) 링크의 10분짜리 동영상으로 잘 설명되어 있으며 테스트를 위한 간단한 flask코드또한 참조할 수 있습니다.
```
your_web_server.py를 작성하였다면 다음을 따르세요
1. your_web_server.py 코드에서 app = flask(name)을 application = app = flask(name)으로 수정하세요.
2. your_web_server.py 파일명을 application.py로 변경하세요.
3. 명령 프롬프트(cmd)를 켠 후 python이 설치된 경로로 이동합니다. 예시) cd c:\users\babab_000\appdata\local\Programs\Python\Python38-32
4. pip freeze 또는 python -m pip freeze명령어를 입력하면 나오는 모듈의 버젼들을 requirements.txt 파일을 만들어 복사 붙여넣기 합니다.
5. application.py파일 , static 폴더, templates 폴더, requirements.txt를 koinvesting.zip으로 압축
6. Elastic Beanstalk 대시보드로 이동 -> 업로드 및 배포 -> 파일 선택(koinvesting.zip) -> 버전 레이블(koinvesting_v1) -> 배포
7. 왼쪽 navbar 에서 환경으로 이동을 누르면 사이트로 이동하여 잘 작동하는 것을 볼 수 있습니다.
```
![python -m pip freeze](https://user-images.githubusercontent.com/37577891/88635185-58e09e00-d0f2-11ea-86c1-cfba7a8f8fdd.PNG)

#### 웹서버 구축까지 완료한 후 일정시간마다 함수를 호출하고 싶을 때는 linux cron작업을 해야 합니다.
##### [linux cron란?](https://www.cyberciti.biz/faq/define-cron-crond-and-cron-jobs/)
```
1. .ebextensions 라는 폴더를 만듭니다.
2. cron-linux.config 파일을 .ebextensions 폴더 안에 넣습니다.
3. cron-linux.config 파일의 */5 * * * * root /usr/local/bin/myscript.sh 는 5분마다 myscript.sh를 호출한다는 의미입니다.
4. cron-linux.config 파일의 /usr/sbin/runuser -l webapp -c 'cd /var/app/current; python3 ./test.py' 는 test.py를 실행시킨다는 의미입니다.
5. test.py 는 현재시간을 불러와 .txt파일에 시간을 저장하는 역할을 수행합니다.
6. .ebextensions폴더 application.py static폴더 templates폴더 requirements.txt test.py를 .zip 압축파일로 저장합니다.
7. .zip파일을 Elastic Beanstalk에 배포합니다.

* cron-linux.config 파일은 저장소에서 다운로드 받을 수 있습니다.
* 명령 프롬프트(cmd)에서 md .ebextensions 라는 명령어를 입력하면 폴더를 생성할 수 있습니다.
* cron-linux.config 파일을 생성하기 위해서는 명령 프롬프트(cmd)에서 copy con cron-linux.config 라는 명령어를 입력한 후 sometext 를 입력  그 이후 Ctrl + Z , Enter를 누르면 저장됩니다. 링크를 참조하시면 도움이 됩니다 : https://m.blog.naver.com/PostView.nhn?blogId=jed00&logNo=140188420401&proxyReferer=https:%2F%2Fwww.google.com%2F
* cron-linux.config 파일을 수정하기 위해서는 명령 프롬프트(cmd)에서 notepad cron-linux.config 라는 명령어를 입력하면 됩니다.
* cron-linux.config 파일을 확인하기 위해서는 명령 프롬프트(cmd)에서 type cron-linux.config 라는 명령어를 입력하면 됩니다.
* 예를 들어 한시간 마다 함수를 호출하고 싶다면 cron-linux.config 파일의 */5 * * * * 부분을 0 */1 * * * 으로 수정하시면 됩니다.
```
#### 하지만 당신의 test.py가 import bs4 와 같은 외부 라이브러리를 포함하고 있다면 추가 작업이 필요합니다.
```
1. 저장소에서 10_commands.config 파일을 다운로드 받습니다.
2. .ebextensions 폴더에 10_commands.config 파일을 넣습니다.
3. notepad 10_commands.config 명령어를 사용하여 편집을 시작합니다.
4. command: pip install bs4 부분에서 bs4를 설치하고싶은 모듈의 이름으로 대체합니다.
5. 예시는 4개의 모듈설치를 하고있습니다. 필요에 따라서 추가 삭제하십시오.
```
----------
#### 시행착오
```
문제1 서버가 실행되다가 메모리부족으로 종료되는 경우가 빈번하여 알아보았더니 서버에서 한 페이지를 호출할때 대규모 크롤링으로 크롤링이 메모리를 많이 잡아먹는다는 것을 알게 됨.
- 해결방안 페이지를 호출할 때가 아닌 일정시간마다 대규모 크롤링을 진행하여 .txt 나 DB에 저장하여 그 값을 불러 옴. 

문제2 Linux Cron을 다루는 방법을 전혀 모르겠었음 검색을 통해서도 한계를 느낌
- 해결방안 stack overflow에 질문을하여 답변을 받음

```
