# docker build -t hgjazhgj/fgo-py:latest .
# adb -a nodaemon server start
# docker run -v ${PWD}:/app/FGO-py -p 5000:5000 --name fgo-py -e ADB_SERVER_SOCKET=tcp:host.docker.internal:5037 -it --rm hgjazhgj/fgo-py

FROM python:3.11-slim
WORKDIR /app/FGO-py/FGO-py
#&& python3.9 -m pip config set global.index-url https://mirrors.aliyun.com/pypi/simple/ \
#&& python3.9 -m pip config set install.trusted-host mirrors.aliyun.com \
#&& apt update \
#&& apt install -y adb \
#&& rm /usr/local/lib/python3.9/site-packages/airtest/core/android/static/adb/linux/adb \
#&& ln -s /usr/bin/adb /usr/local/lib/python3.9/site-packages/airtest/core/android/static/adb/linux/adb \
RUN ln -sf /usr/share/zoneinfo/Asia/Shanghai /etc/localtime \
 && echo 'Asia/Shanghai' > /etc/timezone \
 && pip install airtest tqdm flask pponnxcr pulp\
 && pip uninstall -y opencv-python  opencv-contrib-python opencv-python-headless opencv-contrib-python-headless\
 && pip install opencv-contrib-python-headless \
 && rm -r ~/.cache/pip
CMD ["python", "-X", "utf8", "fgo.py", "cli"]
