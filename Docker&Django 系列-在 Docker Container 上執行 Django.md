
# Docker&Django 系列-在 Docker Container 上執行 Django

## 在 Django 專案的根目錄建立 `Dockerfile` 檔案

#### Dockerfile：包含了所有指令，以组装 Image
```
FROM python:3.8
WORKDIR /app # put application in working directory named 'app'
COPY requirements.txt /app/requirements.txt
RUN pip3 install -r requirements.txt
COPY . . # copy inside the couurent folder to working directory'
CMD ["python3", "manage.py", "runserver", "0.0.0.0:8000"]
```

## Build Image and Run Container
```
> docker build -t <your-dockerhub-username>/<your-project-name>:<tag> .
> docker push <your-dockerhub-username>/<your-project-name>:<tag>

> docker pull <your-dockerhub-username>/<your-project-name>:<tag>
> docker run -p 8000:8000 your-dockerhub-username>/<your-project-name>:<tag> # -p means publish a container’s port(s) to the host
```

參考資料：
* https://www.youtube.com/watch?v=W5Ov0H7E_o4
* https://docs.docker.com/engine/reference/run/