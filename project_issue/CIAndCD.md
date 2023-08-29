# **CI(Continuous Integration) / CD(Continuous Deployment)**

### Dockerfile
```bash
# 기반 이미지 OpenJDK 17로 설정
FROM openjdk:17-jdk-slim

MAINTAINER Kyungtak Park <qkrrudxkr77@naver.com>

# gralde로 빌드된 jar파일을 이미지 내부로 복사
COPY ./app/build/libs/app.jar app.jar 

# Docker 컨테이너가 실행될 때 실행되는 기본 명령어 'java -jar app.jar'
ENTRYPOINT ["java", "-jar", "-Dspring.profiles.active=prod", "app.jar"]
```
> **`Dockerfile`** 은 **컨테이너 이미지를 구성하는 설정 파일입니다.**   
> Gradle로 빌드한 jar 파일을 이미지 내부로 복사하고, 컨테이너가 시작될 때 해당 jar 파일을 실행하는 설정을 정의하였습니다.   
> 이 이미지를 기반으로 **`Docker 컨테이너를 실행하면 애플리케이션이 실행됩니다.`**

### gradle.yml
```shell
# This workflow uses actions that are not certified by GitHub.
# They are provided by a third-party and are governed by
# separate terms of service, privacy policy, and support
# documentation.
# This workflow will build a Java project with Gradle and cache/restore any dependencies to improve the workflow execution time
# For more information see: https://docs.github.com/en/actions/automating-builds-and-tests/building-and-testing-java-with-gradle

# GitHub Repository Actions Tab 에 표시될 Workflow 이름
name: Java CI with Gradle

# 작업을 트리거하는 이벤트
# main 브랜치에 push, pull_request 이벤트 발생시 트리거
on:
  push:
    branches: [ "main" ]
  pull_request:
    branches: [ "main" ]

permissions:
  contents: read

# Workflow는 jobs로 구성
# jobs는 순서대로 또는 병렬로 실행될 수 있음 
jobs:
  # 첫 번째 job 정의 => 이름: build
  build:
    
    # VM의 실행 환경 지정 => GitHub에서 호스팅하는 최신버전의 ubuntu runner에서 실행
    runs-on: ubuntu-latest

    # job을 구성하는 단계, 각 단계는 독립적인 action or shell script
    # 각 단계는 - 기호로 구분하고 리스트 형태로 나열함
    # - run <script>: script 호출
    # - uses <action>: action 호출
    steps:

    # 원격저장소의 코드를 CI 서버가 내려받는 step
    # GitHub이 제공하는 actions/checkout@v3 호출
    - name: Checkout
      uses: actions/checkout@v3
    
    # CI 서버 환경에 JDK17 설치하는 step
    # GitHub이 제공하는 actions/setup-java@v3 호출
    - name: Set up JDK 17
      uses: actions/setup-java@v3
      with:
        java-version: '17'
        distribution: 'temurin'
        
    # CI 서버 환경에 캐싱된 gradle dependency를 복구하는 step
    - name: Cache Depedency Library
      uses: actions/cache@v3.2.6
      with:
        path: |  # 캐싱할 데이터들의 경로
          ~/.gradle/caches
          ~/.gradle/wrapper
          
        # key를 통해 캐싱서버로부터 파일을 복구
        # gradle 파일들을 해싱한 값을 키로 활용
        # gradle 파일에 변화가 생긴다면 해시가 바뀌어 새로운 key로 새롭게 캐시
        key: ${{ runner.os }}-gradle-${{ hashFiles('**/*.gradle*', '**/gradle-wrapper.properties') }}

      # Gradle Build를 위한 권한 부여
    - name: Grant execute permission for gradlew
      run: chmod +x gradlew

    # Gradle clean build를 수행하는 step
    - name: Build with Gradle
      uses: gradle/gradle-build-action@67421db6bd0bf253fb4bd25b31ebb98943c375e1
      with:
        arguments: clean build

      # DockerHub 로그인
    - name: DockerHub Login
      uses: docker/login-action@v2
      with:
        username: ${{ secrets.DOCKERHUB_USERNAME }}
        password: ${{ secrets.DOCKERHUB_PASSWORD }}

      # Docker 이미지 빌드
    - name: Docker Image Build
      run: docker build --platform linux/arm64 -t ${{ secrets.DOCKERHUB_USERNAME }}/${{ secrets.PROJECT_NAME }} .

      # DockerHub Push
    - name: DockerHub Push
      run: docker push ${{ secrets.DOCKERHUB_USERNAME }}/${{ secrets.PROJECT_NAME }}

      # EC2 인스턴스 접속 및 애플리케이션 실행
    - name: Application Run
      uses: appleboy/ssh-action@v0.1.6
      with:
        host: ${{ secrets.EC2_HOST }}
        username: ${{ secrets.EC2_USERNAME }}
        key: ${{ secrets.EC2_KEY }}

        script: |
          sudo docker kill ${{ secrets.PROJECT_NAME }}
          sudo docker rm -f ${{ secrets.PROJECT_NAME }}
          sudo docker rmi ${{ secrets.DOCKERHUB_USERNAME }}/${{ secrets.PROJECT_NAME }}
          sudo docker pull ${{ secrets.DOCKERHUB_USERNAME }}/${{ secrets.PROJECT_NAME }}

          sudo docker run -p ${{ secrets.PORT }}:${{ secrets.PORT }} \
          --name ${{ secrets.PROJECT_NAME }} \
          -e DB_URL=${{ secrets.DB_URL }} \
          -e DB_USERNAME=${{ secrets.DB_USERNAME }} \
          -e DB_PASSWORD=${{ secrets.DB_PASSWORD }} \
          -e IAMPORT_KEY=${{ secrets.IAMPORT_KEY }} \
          -e IAMPORT_SECRET=${{ secrets.IAMPORT_SECRET }} \
          -d ${{ secrets.DOCKERHUB_USERNAME }}/${{ secrets.PROJECT_NAME }}
```
> 각 속성의 역할은 주석의 내용과 같습니다.   
> 보안이 필요한 값들은 **`${{ secrets.키값 }}`** 와 같이 표현 하였으며, 실제 키와 밸류는 Github Repository의 Actions secrets and variables 페이지에 저장해 놓았습니다.