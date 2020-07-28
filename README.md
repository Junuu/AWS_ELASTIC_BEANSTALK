#### 아마존 웹서비스 - Elastic Beanstalk 처음시작부터 배포해보기 ( A to Z)
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
4. pip freeze 또는 python -m pip freeze명령어를 입력하면 나오는 모듈의 버젼들을 requirements.txt 파일을 만들어 저장합니다.
5. application.py파일 , static 폴더, templates 폴더, requirements.txt를 koinvesting.zip으로 압축
6. Elastic Beanstalk 대시보드로 이동 -> 업로드 및 배포 -> 파일 선택(koinvesting.zip) -> 버전 레이블(koinvesting_v1) -> 배포
7. 왼쪽 navbar 에서 환경으로 이동을 누르면 사이트로 이동하여 잘 작동하는 것을 볼 수 있습니다.
```
![python -m pip freeze](https://user-images.githubusercontent.com/37577891/88635185-58e09e00-d0f2-11ea-86c1-cfba7a8f8fdd.PNG)

