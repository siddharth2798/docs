# MobSF Docker विकल्प

!> डॉकर के साथ डायनेमिक विश्लेषण समर्थित नहीं है |
?> आप यहां बिना किसी सेटअप के सीधे Docker Image का उपयोग कर सकते हैं [DockerHub](https://hub.docker.com/r/opensecurity/mobile-security-framework-mobsf/).

**DockerHub से पूर्वनिर्मित Docker Image का उपयोग**

```bash
docker pull opensecurity/mobile-security-framework-mobsf
# केवल स्थैतिक विश्लेषण
docker run -it --rm -p 8000:8000 opensecurity/mobile-security-framework-mobsf:latest
```

**दृढ़ता के लिए**

```bash
mkdir <your_local_dir>
chown 9901:9901 <your_local_dir>
docker run -it --rm --name mobsf -p 8000:8000 -v <your_local_dir>:/home/mobsf/.MobSF opensecurity/mobile-security-framework-mobsf:latest
```

**Dockerfile से Docker Image का निर्माण**

```bash
git clone https://github.com/MobSF/Mobile-Security-Framework-MobSF.git
cd Mobile-Security-Framework-MobSF
docker build -t mobsf .
docker run -it --rm -p 8000:8000 mobsf
```

**Dockerfile से प्रॉक्सी का उपयोग करने वाली Image बनाना**

```bash
docker build --build-arg https_proxy="https://${PROXY_IP}:${PROXY_PORT}" --build-arg http_proxy="${PROXY_IP}:${PROXY_PORT}" --build-arg NO_PROXY="127.0.0.1" -t mobsf .
```

`PROXY_IP` environment variable का मान proxy ip address रखें और `PROXY_PORT` का मान proxy port रखें.

**Dockerfile से Image का पुनर्निर्माण**

```bash
docker rmi ubuntu:18.04
docker build --no-cache --rm -t mobsf .
```

**postgres अवलंब**

आपको docker-compose की आवश्यकता होगी, (देखे: <https://docs.docker.com/compose/install>).

* Images का निर्माण करें 

`docker-compose build`

* सेवाओं को शुरू करें

`docker-compose up -d`  (पृष्ठभूमि में)

or

`docker-compose up ` (पृष्ठभूमि में)

फिर पुष्टि करें कि 2 सेवाएं काम कर रही हैं:

`docker ps`

```bash
CONTAINER ID        IMAGE                                   COMMAND                  CREATED             STATUS              PORTS                          NAMES
7de107c5b853        mobile-security-framework-mobsf_mobsf   "python3 manage.py r…"   5 weeks ago         Up 5 weeks          0.0.0.0:8000->8000/tcp         mobile-security-framework-mobsf_mobsf_1
149a3ffa61ca        postgres:latest                         "docker-entrypoint.s…"   5 weeks ago         Up 5 weeks          5432/tcp                       mobile-security-framework-mobsf_postgres_1
```

**यदि इसे `-d` के बजाय `-it` के साथ शुरू किया गया हो तो आप यहां देख सकते हैं कि container के अंदर क्या हुआ**

```bash
docker logs -f --tail 100 mobsf
```

**कंटेनर में shell का उपयोग करने के लिए**

```bash
docker exec -it mobsf /bin/bash
```
