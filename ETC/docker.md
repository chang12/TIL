[docker 홈페이지](https://www.docker.com/community-edition) 에서 Mac 용 dmg 파일을 다운로드 받고, docker 를 설치한다. image 와 container 의 관계는 OOP 에서 class 와 instance 의 관계와 유사한것 같다. [Getting started](https://docs.docker.com/get-started/) 를 찬찬히 따라가보는데 hello-world 대신 ubuntu image 를 사용해본다.

* **docker**: container 로 develop, deploy, run applications platform.
* **container**: a runtime instance of an image.
* **image**: code + runtime + libraries + environment variables + configuration files.
* container -> service -> stack hierarchy.

vm 은 process 마다 guest os 가 함께 뜨고 hypervisor 를 통해 host os 의 resource 에 접근하는데 비해, docker 는 host os 의 resource 를 여러 process 가 함께 공유한다.

```sh
# NAME:TAG 를 명시해서 ubuntu 14.04 image 를 다운로드
docker pull ubuntu:14.04

# launch container in background and keep STDIN open
docker run -id ubuntu:14.04

# check container id
docker ps

# open bash shell
docker exec -i <container_id> bash
```

