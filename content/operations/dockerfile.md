## 处理关于Dockerfile的相关记录

> 记录一下普通的`Dockerfile`长什么样儿。

```
FROM pig4cloud/java:8-jre

MAINTAINER wangiegie@gmail.com

ENV TZ=Asia/Shanghai
ENV JAVA_OPTS="-Xms512m -Xmx1024m -Djava.security.egd=file:/dev/./urandom"

RUN ln -sf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone

RUN mkdir -p /pigx-upms

WORKDIR /pigx-upms

EXPOSE 4000

ADD ./target/pigx-upms-biz.jar ./

CMD sleep 60;java $JAVA_OPTS -jar pigx-upms-biz.jar
```