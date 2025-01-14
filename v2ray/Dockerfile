FROM alpine:3.8

LABEL maintainer="yetio <yetio@126.com>"

ARG TZ="Asia/Shanghai"

ENV TZ ${TZ}
ENV V2RAY_VERSION v4.31.0
ENV V2RAY_LOG_DIR /var/log/v2ray
ENV V2RAY_CONFIG_DIR /etc/v2ray/
ENV V2RAY_DOWNLOAD_URL https://github.com/v2fly/v2ray-core/releases/download/${V2RAY_VERSION}/v2ray-linux-64.zip

RUN set -eux \
    && sed -i 's/dl-cdn.alpinelinux.org/mirrors.ustc.edu.cn/g' /etc/apk/repositories \
    && apk upgrade --update \
    && apk add \
        bash \
        tzdata \
        curl \
    && mkdir -p \ 
        ${V2RAY_LOG_DIR} \
        ${V2RAY_CONFIG_DIR} \
        /tmp/v2ray \
    && curl -L -H "Cache-Control: no-cache" -o /tmp/v2ray/v2ray.zip ${V2RAY_DOWNLOAD_URL} \
    && unzip /tmp/v2ray/v2ray.zip -d /tmp/v2ray/ \
    && mv /tmp/v2ray/v2ray /usr/bin \
    && mv /tmp/v2ray/v2ctl /usr/bin \ 
    && mv /tmp/v2ray/geoip /usr/bin \ 
    && mv /tmp/v2ray/geosite /usr/bin \ 
    && mv /tmp/v2ray/vpoint_vmess_freedom.json /etc/v2ray/config.json \
    && chmod +x /usr/bin/v2ray \
    && apk del curl \
    && ln -sf /usr/share/zoneinfo/${TZ} /etc/localtime \
    && echo ${TZ} > /etc/timezone \
    && rm -rf /tmp/v2ray /var/cache/apk/*

ADD entrypoint.sh /entrypoint.sh

ENTRYPOINT ["/entrypoint.sh"]
