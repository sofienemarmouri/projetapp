FROM nginx:alpine
VOLUME [ "/var/log" ]
RUN echo "hello world" > /var/log/test
#WORKDIR /usr/share/nginx/html
#COPY index.html index.html

COPY index.html /usr/share/nginx/html/index.html