FROM nginx:1.12
LABEL maintainer="gzp@goozp.com"

# set timezome
ENV TZ=Asia/Shanghai
ENV LANG C.UTF-8
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone