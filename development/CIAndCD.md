# **CI(Continuous Integration) / CD(Continuous Deployment)**

```bash
# 기반 이미지 OpenJDK 17로 설정
FROM openjdk:17-jdk-slim

MAINTAINER Kyungtak Park <qkrrudxkr77@naver.com>

# gralde로 빌드된 jar파일을 이미지 내부로 복사
COPY ./app/build/libs/app.jar app.jar 

# Docker 컨테이너가 실행될 때 실행되는 기본 명령어 'java -jar app.jar'
ENTRYPOINT ["java", "-jar", "-Dspring.profiles.active=prod", "app.jar"]
```
> Dockerfile은 컨테이너 이미지를 구성하는 설정 파일입니다.   
> Gradle로 빌드한 jar 파일을 이미지 내부로 복사하고, 컨테이너가 시작될 때 해당 jar 파일을 실행하는 설정을 정의하였습니다.   
> 이 이미지를 기반으로 Docker 컨테이너를 실행하면 애플리케이션이 실행됩니다.