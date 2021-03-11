FROM node:14.16-alpine as builder

RUN apk add --no-cache git=2.24.4-r0 \
 && npm install -g @vue/cli@4.5.11 \
 && npm install -g @vue/cli-service-global@4.5.11 \
 && mkdir /build

COPY . /build/

RUN cd /build \
 && npm install \
 && npm run build

FROM nginx:1.19.7-alpine

COPY --from=builder /build/dist/. /usr/share/nginx/html/
