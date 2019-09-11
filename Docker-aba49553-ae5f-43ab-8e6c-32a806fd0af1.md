# Docker로 서버환경 구축하기

이제 딥러닝 모델을 사용한 서버 배포하기 마지막 단계입니다!

도커를 사용해서 이전 포스팅에서 만들었던 Flask서버를 관리하고 배포해보겠습니다!

> 도커에 대해 전혀 모르시는 분은 [이 포스팅](https://subicura.com/2017/01/19/docker-guide-for-beginners-1.html)을 꼭 읽어보시기 바랍니다!



## 도커 시작하기

![](/home/hyunkyung/docker.png)

도커를 사용하는 방식은 크게 두 가지 방식이 있습니다.

1. 기존에 있는 이미지를 사용 (run)
2. 이미지를 빌드 후 사용 (build -> run)

보통은 ``OS``나 ``Programming Language`` 와 같이 기본적인 이미지를 가져다가 사용하고, 거기에 환경설정등을 추가해 새로운 이미지를 빌드해 사용합니다.



이번 포스팅에서는 

1. 기존 이미지 사용하기
2. 이미지 빌드 후 사용하기

에 대한 기본적인 내용을 살펴보고, 다음 포스팅에서 실제 Flask 서버를 배포하는 방법을 다루겠습니다.



### 1. 기존 이미지 사용하기

###  

**이미지 받아오기**

[DockerHub](https://hub.docker.com/search?q=&type=image) 에는 공식 이미지부터 개인이 커스텀한 이미지까지 매우 다양한 이미지들이 올라가 있습니다.

![](/home/hyunkyung/docker2.png)

여기서 원하는 이미지를 선택해 들어가면 

![](/home/hyunkyung/docker3.png)

이렇게 command를 복사해서 사용할 수 있게 되어있습니다!

그대로 복사해서 ``docker pull python``을 입력하면 python 이미지를 받아올 수 있고,<br>``docker images`` 를 입력하면 저장된 이미지 목록을 알 수 있습니다.



**이미지 실행하기**

이제 받아온 이미지를 실행하면 컨테이너를 생성할 수 있습니다.

```bash
docker run [OPTIONS] [IMAGE]
```

앞에서 받아온 python 이미지를 실행하려면 ``docker run python`` 이라고 입력하면 되겠죠?



이미지를 실행할 때 여러가지 옵션을 사용하는데, 그 중 자주 쓰이는 옵션들을 알아보겠습니다.

> 옵션에 대한 자세한 설명은 [여기](http://pyrasis.com/book/DockerForTheReallyImpatient/Chapter20/28)를 참고하세요!

- -it

  : 컨테이너에 터미널 형태로 접근하고 싶을 때 사용합니다. 

  - -t : bash 형태 사용
  - -i : 표준 입력 (키보드 입력) 사용 

- --rm

  : 컨테이너 내의 프로세스가 종료되면 컨테이너를 자동으로 삭제합니다.

- -d

  : 백그라운드 모드로 실행합니다. 

- -p

  : 호스트와 컨테이너의 포트를 연결합니다.

  - ``<호스트 포트>:<컨테이너 포트>`` (ex. ``-p 5000:5000``)

- --name

  : 컨테이너 이름을 설정합니다.

- -v 

  : 컨테이너 내 경로와 호스트 내 경로를 마운트합니다.

  - ``<``



### 도커 실행하기

    docker run [OPTIONS] [IMAGE]

option ([http://pyrasis.com/book/DockerForTheReallyImpatient/Chapter20/28](http://pyrasis.com/book/DockerForTheReallyImpatient/Chapter20/28))

- -t : bash 형태 사용
- -i : 표준 입력 사용 (키보드 입력)
- —rm : 컨테이너 안의 프로세스가 종료되면 컨테이너를 자동으로 삭제
- -d : 백그라운드 모드로 실행 (foreground로 실행하면 컨테이너 실행하는 동안 다른 명령을 할 수 없음)
- -p : 호스트에 연결된 컨테이너의 특정 포트를 외부에 노출, 보통 웹 서버의 포트를 노출
    - `<호스트 포트>:<컨테이너 포트>`
    - `<IP 주소>:<호스트 포트>:<컨테이너 포트>` 호스트에 네트워크 인터페이스가 여러 개이거나 IP 주소가 여러 개 일 때 사용합니다. 예) *-p 192.168.0.10:80:80*
    - `<IP 주소>::<컨테이너 포트>` 호스트 포트를 설정하지 않으면 호스트의 포트 번호가 무작위로 설정됩니다. 예) *-p 192.168.0.10::80*
    - `<컨테이너 포트>` 컨테이너 포트만 설정하면 호스트의 포트 번호가 무작위로 설정됩니다. 예) *-p 80*
- -e : 환경변수 설정
- —name : 컨테이너 이름 설정
- -v : 컨테이너 내 경로와 호스트 내 경로를 마운트
    - 사용 예
        1. 호스트 디렉터리에서 수정한 코드를 컨테이너에 반영
        2. 컨테이너 내에서 생성된 파일을 호스트 디렉터리에 저장
    - `<호스트 디렉터리>:<컨테이너 디렉터리>` 예) *-v /data:/data*
    - `<호스트 디렉터리>:<컨테이너 디렉터리>:<ro, rw>` 예) *-v /data:/data:ro (read only, read write)*
    - `<호스트 파일>:<컨테이너 파일>` 예) *-v /var/run/docker.sock:/var/run/docker.sock*



### 컨테이너 확인

**확인**

    docker ps #동작중인 컨테이너 확인
    docker ps -a #중지된 컨테이너 확인
    docker ps -a -q #중지된 컨테이너의 ID값만 확인

**삭제**

    docker stop [container ID] #컨테이너 중지 (중지된 컨테이너만 삭제 가능)
    docker rm [container ID]
    docker rm [container ID 1], [container ID 2] #여러개 삭제
    docker rm `docker ps -a -q` #모든 컨테이너 삭제

**로그 보기**

    docker logs [container ID]
    
    # -tail : 마지막 10줄만 출력
    # -f : 실시간 확인

- 이 때 출력되는 오류는 컨테이너 내 stdout, stderr를 보여주는 것
- 기본적으로 json 방식으로 로그를 저장 (너무 커질 수 있으므로 주의)

**명령어 실행**

실행중인 컨테이너의 파일을 실행

    docker exec 
    docker exec -it /bin/bash #컨테이너 내에서 bash 창에서 키보드 입력을 사용



### 이미지 확인

**확인**

    docker images

**삭제**

    docker rmi [image ID]
    docker rmi -f [image ID] #중지되지 않은 컨테이너의 이미지를 삭제하는 경우 컨테이너도 강제 삭제
    
    docker system prune -a #모두 삭제,,,

**다운로드**

    docker pull [image NAME]



### 도커 컴포즈 사용하기

[https://www.44bits.io/ko/post/almost-perfect-development-environment-with-docker-and-docker-compose#도커-컴포즈로-개발-환경-구성하기](https://www.44bits.io/ko/post/almost-perfect-development-environment-with-docker-and-docker-compose#%EB%8F%84%EC%BB%A4-%EC%BB%B4%ED%8F%AC%EC%A6%88%EB%A1%9C-%EA%B0%9C%EB%B0%9C-%ED%99%98%EA%B2%BD-%EA%B5%AC%EC%84%B1%ED%95%98%EA%B8%B0)

`docker-compose.yml` 파일 생성 후 아래 내용 작성

    version: '3' # 파일 규격 옵션 (3으로 시작하는 최신 버전 사용)
    
    volumes:
    	django_sample_db_dev: {} # 볼륨 생성 (도커가 관리하는 가상 디스크)
    
    services:
      db: # 실행할 컨테이너 이름
        image: postgres # 사용할 이미지 (이미지 파일 자체를 사용)
        volumes:
    			- django_sample_db_dev:/var/lib/postgresql/data
          #- ./docker/data:/var/lib/postgresql/data
        environment:
          - POSTGRES_DB=sampledb
          - POSTGRES_USER=sampleuser
          - POSTGRES_PASSWORD=samplesecret
          - POSTGRES_INITDB_ARGS=--encoding=UTF-8
    
      django: # 실행할 컨테이너 이름2 (순서대로 실행)
        build: # 이미지 대신 build해 사용
          context: . # build 명령을 실행할 경로
          dockerfile: ./compose/django/Dockerfile-dev # 이미지 빌드를 위해 사용할 Dockerfile
        environment:
          - DJANGO_DEBUG=True
          - DJANGO_DB_HOST=db
          - DJANGO_DB_PORT=5432
          - DJANGO_DB_NAME=sampledb
          - DJANGO_DB_USERNAME=sampleuser
          - DJANGO_DB_PASSWORD=samplesecret
          - DJANGO_SECRET_KEY=dev_secret_key
        ports:
          - "8000:8000"
        command: 
          - python manage.py runserver 0:8000
        volumes:
          - ./:/app/

docker-compose 파일 내 서비스? 들은 같은 네트워크에 속하기 때문에 별도의 -link 옵션이 필요 없음

- [ ]  docker service VS container ??



### 이미지 생성하기

`Dockerfile` 을 작성해야 함

    FROM python # 기본 이미지로 python을 사용 (옵션이 없으므로 python:latest)
    COPY . /app # 현재 디렉토리의 내용을 /app 폴더로 복사
    WORKDIR /app # /app 폴더를 기본 디렉토리로 사용
    RUN pip install -r requirements.txt # 필요한 파일을 설치
    EXPOSE 5000 # 5000번 포트를 열어둠 
    CMD ["python", "app.py"] # python app.py 실행

- FROM

    : 베이스 이미지 지정 (필수), tag는 버전을 의미 (기본값은 latest)

        FROM <image>:<tag>
        
        # ex)
        FROM ubuntu:16.04 # 기본 이미지로 ubuntu 16.04를 사용 

- COPY

    : 파일 / 디렉토리를 이미지로 복사

        COPY <src><dest>
        
        # ex) 
        COPY . /app # 현재 경로의 파일들을 이미지 내 /app 폴더로 복사

- ADD

    : COPY와 유사

    - src에 URL입력 가능
    - src에 압축파일 입력 시 자동으로 압축 해제

- RUN

    : 명령어를 그대로 실행

        RUN <command>
        
        # ex)
        RUN ["executable", "param1", "param2"] # 실행파일에 parameter 지정해 실행
        RUN pip install -r requirements.txt # 명령어 실행

- CMD

    : 도커 컨테이너 실행 시 실행되는 명령어

        CMD <command>
        
        # ex)
        CMD ["executable", "param1", "param2"] # 실행파일에 parameter 지정해 실행
        CMD ["python", "app.py"] # python 실행파일로 app.py를 실행
        CMD command param1 param2 # 명령어에 parameter 지정해 실행

- WORKDIR

    : RUN, ADD 등이 이루어질 기본 디렉토리 설정

        WORKDIR <path>
        
        # ex)
        COPY . /app
        WORKDIR /app
        # 현재 디렉토리의 내용을 app 폴더에 복사 후, 기본 디렉토리를 app 폴더로 지정
        # 이후의 명령어는 app 폴더를 기본 디렉토리로 사용

- EXPOSE

    : 도커 컨테이너 실행 시 요청을 기다리는 포트를 지정

        EXPOSE <port>

- VOLUME

    : 컨테이너 외부에 파일 시스템을 마운트

    - 컨테이너 실행 할 때 `-v` 옵션 사용시 `-v` 우선 적용?

        VOLUME <volume name>

- ENV

    : 컨테이너에서 사용할 환경변수를 지정

    - 컨테이너 실행할 때 `-e` 옵션 사용시 `-e` 우선 적용 (오버라이드)

        ENV <key><value>

**이미지 빌드**

    docker build -t [image Name] .

# Flask 앱을 실행할 이미지 만들기

과정

1. 이미지 파일 작성 (Dockerfile)
2. 이미지 파일 빌드
3. 이미지 파일 실행
4. docker-compose 이용해 실행 옵션 저장

## 1. 이미지 파일 작성

`Dockerfile`

    # 1. 기본 이미지 설정
    FROM python:3.6
    
    # 2. 현재 디렉토리 내용을 이미지 파일 내 디렉토리에 복사
    COPY . /app
    
    # 3. 기본 디렉토리 지정
    WORKDIR /app
    
    # 4. 필요한 패키지 설치
    RUN pip install -r requirements.txt
    
    # 5. 포트 열기 (listen 할 준비)
    EXPOSE 5000
    
    # 6. app.py 실행
    CMD ["python", "app.py"]

위의 파일대로 작성하면 [`app.py`](http://app.py) 파일을 수정할 때 마다 #4 필요한 패키지를 설치해야 하는 문제가 있음

Dockerfile은 위에서부터 차례대로 캐싱된 임시 이미지 파일을 사용하기 때문에,

자주 바뀌지 않는 과정 (ex. 패키지 설치)는 캐싱된 값을 사용하는 것이 좋음 

`Dockerfile`

    # 1. 기본 이미지 설정
    FROM python:3.6
    
    # 2. 캐싱된 패키지 목록을 사용하기 위해 requirement 먼저 복사
    RUN mkdir /app
    COPY requirements.txt /app
    
    # 3. 기본 디렉토리 지정
    WORKDIR /app
    
    # 4. 필요한 패키지 설치
    RUN pip install -r requirements.txt
    
    # 5. 나머지 디렉토리도 복사 (수정한 app.py 파일 포함)
    COPY . /app
    
    # 6. 포트 열기
    EXPOSE 5000
    
    # 7. app.py 실행
    CMD ["python", "app.py"]

**주의)**

[`app.py`](http://app.py) 에서 app.run() 할 때 host를 명시해주지 않으면 정상 작동하지 않음

    if __name__ == "__main__":
    
    	#app.run(debug=True) 이렇게 하면 정상 작동하지 않음
    	app.run(debug=True, host='0.0.0.0') # host 명시

## 2. 이미지 파일 빌드

작성한 `Dockerfile` 을 `vip` 라는 이미지로 빌드 (-t : 이미지 이름을 지정하는 옵션)

    docker build -t vip .

## 3. 이미지 파일 실행

빌드한 `vip` 이미지를 실행

    docker run -d -p 5000:5000 vip
    
    # -p 5000:5000 : local 포트 5000을 이미지 내 포트 5000으로 연결
    # -d : 백그라운드로 실행 

실행 결과는 `127.0.0.1:5000` 에서 확인 가능 

## 4. docker-compose 사용하기

위에서 `docker run` 을 하려면 옵션도 써줘야 하고, vip라는 이미지 파일을 빌드한 후에 사용할 수 있음

옵션과 이미지 빌드를 한번에 관리하기 위해 docker-compose를 사용

`docker-compose.yml` 파일 작성

    version: '3'
    
    services:
      flask:
        build: # 이미지 빌드 설정
          context: .
          dockerfile: ./Dockerfile-dev
    		
    		# 실행 옵션 설정
        ports:
          - "5000:5000"
        command:
          - bash
          - -c
          - |
            python app.py

- [ ]  command에서 `python app.py` 만 쓰면 Cannot start service flask: OCI runtime create failed 문제가 발생하는 이유?

docker-compose 내에서 빌드할 파일은 개발용으로 따로 작성 ~~하는것이 좋음~~

위에서 작성한 Dockerfile 에는 docker-compose와 같은 역할을 하는 내용들이 있기 때문

`Dockerfile-dev`

    # 1. 기본 이미지 설정
    FROM python:3.6
    
    # 2. 캐싱된 패키지 목록을 사용하기 위해 requirement 먼저 복사
    RUN mkdir /app
    COPY requirements.txt /app
    
    # 3. 기본 디렉토리 지정
    WORKDIR /app
    
    # 4. 필요한 패키지 설치
    RUN pip install -r requirements.txt
    
    # 5. 나머지 디렉토리도 복사 (수정한 app.py 파일 포함)
    COPY . /app
    
    # 6. 포트 열기
    #EXPOSE 5000
    
    # 7. app.py 실행
    #CMD ["python", "app.py"]

#6 포트 열기와 #7 파일 실행은 docker-compose에서 관리하기 때문에 주석처리

    docker-compose up

실행하면 이미지 빌드와 실행 둘 다 처리되는 것을 확인할 수 있음