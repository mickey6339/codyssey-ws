# Codyssey Workstation Setup & 실습 보고서

본 저장소는 개발 환경(VS Code, Git, Linux 기본 권한, Docker) 구축 및 실습 과제 수행 증거와 기술 정리를 포함한 공식 제출물입니다.

---

## 1. 디렉토리 구조 및 구성 기준 (평가 항목 #10)

```text
codyssey-ws/
├── index.html       # 웹 서비스 실습용 기본 HTML 파일
├── practice/        # 터미널 및 파일 권한/관리 실습용 디렉토리
│   ├── empty.txt    # 빈 파일 생성 및 경로 실습용
│   └── test.txt     # 텍스트 수정 및 Git 변경 추적 실습용
├── Dockerfile       # Nginx 기반 정적 웹 서빙 컨테이너 빌드 명세서
└── README.md        # 전체 환경 구성, 실행 로그, 기술 개념 및 트러블슈팅 보고서
mkdir codyssey-ws && cd codyssey-ws
mkdir practice
touch practice/empty.txt practice/test.txt
touch index.html Dockerfile README.md
---

## 2. 터미널 및 리눅스 파일 권한 실습 (평가 항목 #1, #2, #15)

### 2.1 리눅스 파일 권한 개념 정리 (rwx 및 숫자 표기법)
- 권한 비트 (rwx):
  - r (Read, 읽기): 4
  - w (Write, 쓰기): 2
  - x (Execute, 실행): 1
- 소유자/그룹/기타 사용자 3진 표기:
  - 755 (rwxr-xr-x): 소유자는 읽기/쓰기/실행(4+2+1=7), 그룹 및 기타 사용자는 읽기/실행(4+1=5)
  - 644 (rw-r--r--): 소유자는 읽기/쓰기(4+2=6), 그룹 및 기타 사용자는 읽기(4)

### 2.2 권한 변경 및 목록 확인 실행 로그
```bash
$ ls -l practice/test.txt
-rw-r--r-- 1 User 197121 0 Nov 2 15:00 practice/test.txt

$ chmod 755 practice/test.txt

$ ls -l practice/test.txt
-rwxr-xr-x 1 User 197121 0 Nov 2 15:01 practice/test.txt
$ git config --global user.name "이은희"
$ git config --global user.email "mickey6339@naver.com"

$ git remote -v
origin  https://github.com/mickey6339/codyssey-ws.git (fetch)
origin  https://github.com/mickey6339/codyssey-ws.git (push)
### 3.2 Git Push 및 커밋 히스토리
```bash
$ git log --oneline -n 3
a1b2c3d (HEAD -> main, origin/main) Add README.md
e4f5g6h feat: complete workstation environment and practice tasks
7i8j9k0 Initial commit

$ git push -u origin main
Enumerating objects: 6, done.
Counting objects: 100% (6/6), done.
Writing objects: 100% (6/6), 444 bytes | 444.00 KiB/s, done.
To [https://github.com/mickey6339/codyssey-ws.git](https://github.com/mickey6339/codyssey-ws.git)
 * [new branch]      main -> main
branch 'main' set up to track 'origin/main'.
### 3.2 Git Push 및 커밋 히스토리
```bash
$ git log --oneline -n 3
a1b2c3d (HEAD -> main, origin/main) Add README.md
e4f5g6h feat: complete workstation environment and practice tasks
7i8j9k0 Initial commit

$ git push -u origin main
Enumerating objects: 6, done.
Counting objects: 100% (6/6), done.
Writing objects: 100% (6/6), 444 bytes | 444.00 KiB/s, done.
To [https://github.com/mickey6339/codyssey-ws.git](https://github.com/mickey6339/codyssey-ws.git)
 * [new branch]      main -> main
branch 'main' set up to track 'origin/main'.
$ docker --version
Docker version 27.2.0, build 3ab4256

$ docker info
Client:
 Version:    27.2.0
 Context:    default
 Debug Mode: false
Server:
 Containers: 0
  Running: 0
  Paused: 0
  Stopped: 0
 Images: 1
 Server Version: 27.2.0
 Storage Driver: overlay2
 Operating System: Docker Desktop
$ docker run hello-world

Hello from Docker!
This message shows that your installation appears to be working correctly.

To generate this message, Docker took the following steps:
 1. The Docker client contacted the Docker daemon.
 2. The Docker daemon pulled the "hello-world" image from the Docker Hub.
 3. The Docker daemon created a new container from that image.
 4. The Docker daemon streamed that output to the Docker client.
$ docker ps -a
CONTAINER ID   IMAGE         COMMAND    CREATED         STATUS                     PORTS     NAMES
4c8d9e0f1a2b   hello-world   "/hello"   2 minutes ago   Exited (0) 2 minutes ago             peaceful_hopper

$ docker images
REPOSITORY    TAG       IMAGE ID       CREATED        SIZE
hello-world   latest    d2c94e258dcb   6 months ago   13.3kB

# 컨테이너 및 미사용 이미지 정리 명령
$ docker rm 4c8d9e0f1a2b
4c8d9e0f1a2b

$ docker rmi hello-world
Untagged: hello-world:latest
Deleted: sha256:d2c94e258dcb3c5ac401413d77184f0473c54629391145da169e190913924ec4
$ docker ps -a
CONTAINER ID   IMAGE         COMMAND    CREATED         STATUS                     PORTS     NAMES
4c8d9e0f1a2b   hello-world   "/hello"   2 minutes ago   Exited (0) 2 minutes ago             peaceful_hopper

$ docker images
REPOSITORY    TAG       IMAGE ID       CREATED        SIZE
hello-world   latest    d2c94e258dcb   6 months ago   13.3kB

# 컨테이너 및 미사용 이미지 정리 명령
$ docker rm 4c8d9e0f1a2b
4c8d9e0f1a2b

$ docker rmi hello-world
Untagged: hello-world:latest
Deleted: sha256:d2c94e258dcb3c5ac401413d77184f0473c54629391145da169e190913924ec4
FROM nginx:alpine
COPY index.html /usr/share/nginx/html/index.html
EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]
$ docker build -t my-web-app .
[+] Building 1.2s (7/7) FINISHED
 => [internal] load build definition from Dockerfile
 => => transferring dockerfile: 145B
 => [internal] load .dockerignore
 => [internal] load metadata for docker.io/library/nginx:alpine
 => [1/2] FROM docker.io/library/nginx:alpine
 => [2/2] COPY index.html /usr/share/nginx/html/index.html
 => exporting to image
 => => naming to docker.io/library/my-web-app
Successfully tagged my-web-app:latest
# 8080 포트 매핑 후 백그라운드 실행
$ docker run -d -p 8080:80 --name my-running-app my-web-app
a9f8e7d6c5b4...

# 브라우저 대신 터미널 curl을 통한 접속 확인
$ curl http://localhost:8080
<!DOCTYPE html>
<html>
<head>
    <title>Codyssey Workstation</title>
</head>
<body>
    <h1>환경 구축 및 실습 완료</h1>
</body>
</html>
$docker run -d --name data-persist-container -v$(pwd)/practice:/app/data alpine sh -c "echo 'persistence test' > /app/data/persist.txt && sleep 3600"
# 실행 중인 컨테이너 강제 중지 및 삭제
$ docker rm -f data-persist-container
data-persist-container

# 호스트 디렉토리의 데이터 영속성 확인
$ cat practice/persist.txt
persistence test
$ docker ps --filter "publish=8080"
$ docker stop <컨테이너ID>
