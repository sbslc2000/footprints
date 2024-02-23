Travis CI는 Github에서 진행되는 오픈소스 프로젝트를 위한 지속적인 통합(Continuous Integration) 서비스이다. 2011년에 설립되어 2012년에 급성장하였으며 많은 개발 언어를 지원한다. Travis CI를 이용하면 깃허브 저장소에 있는 프로젝트를 특정 이벤트에 따라 자동으로 테스트, 빌드하거나 배포할 수 있다. Private Repository는 유료로 일정 금액을 지불하고 사용할 수 있다.

# 흐름
1. 로컬 Git에 있는 소스를 Github 저장소에 Push를 한다.
2. Github master 저장소에 소스가 Push 되면 Travis CI에게 소스가 PUSH 되었다고 얘기를 해줍니다.
3. Travis CI는 업데이트된 소스를 Github에서 가지고 옵니다.
4. 깃헙에서 가져온 소스의 테스트 코드를 실행해 봅니다.
5. 테스트 코드 실행 후 테스트가 성공하면 호스팅 사이트로 보내서 배포를 합니다.

## .travis.yml
```yml
sudo: # 관리자 권한 여부
language: # 언어(플랫폼) 선택
services: # 환경 구성 (Docker)
before_install: # 스크립트 실행할 수 있는 환경 구성 (Docker 이미지 생성)
script: #실행할 스크립트 (Docker를 통해테스트 실행)
after_success: # 테스트 성공 후 할 일
```