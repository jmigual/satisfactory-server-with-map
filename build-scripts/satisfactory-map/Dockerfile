FROM nginx

ARG USER_DOMAIN
ENV USER_DOMAIN $USER_DOMAIN

COPY nginx.conf /nginx.conf.tmpl
RUN envsubst < /nginx.conf.tmpl > /etc/nginx/nginx.conf
