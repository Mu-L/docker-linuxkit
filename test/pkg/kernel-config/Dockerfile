FROM linuxkit/alpine:35b33c6b03c40e51046c3b053dd131a68a26c37a AS mirror

RUN mkdir -p /out/etc/apk && cp -r /etc/apk/* /out/etc/apk/
RUN apk add --no-cache --initdb -p /out alpine-baselayout busybox musl bash

ENV MOBY_CHECK_CONFIG_COMMIT=38005cfc12fb6bdcff359c25e8fe05f2944b0d49
ADD https://raw.githubusercontent.com/moby/moby/${MOBY_CHECK_CONFIG_COMMIT}/contrib/check-config.sh /out/check-config.sh
ADD . ./out

RUN mkdir -p /out/lib/modules

FROM scratch
COPY --from=mirror /out /
ENTRYPOINT ["/bin/sh", "/check.sh"]
LABEL org.mobyproject.config='{"readonly": true, "binds": ["/lib/modules:/lib/modules", "/dev:/dev", "/sys:/sys"], "capabilities": ["all"]}'
