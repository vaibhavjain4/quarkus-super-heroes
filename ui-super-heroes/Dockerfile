FROM nginxinc/nginx-unprivileged

#### copy nginx conf
COPY ./nginx/*.conf /etc/nginx/conf.d/

USER 1001

COPY --chown=1001:root . /usr/share/nginx/html

#### don't know what this is, but seems cool and techy
CMD ["nginx", "-g", "daemon off;"]
