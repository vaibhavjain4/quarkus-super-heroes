### STAGE 1: Build ###
FROM node:lts-alpine AS build

USER 1001


#### make the 'app' folder the current working directory
WORKDIR /usr/src/app

#RUN chown 1001 /usr/src/app \
    && chmod "g+rwX" /usr/src/app \
    && chown 1001:root /usr/src/app

#### copy both 'package.json' and 'package-lock.json' (if available)
COPY --chown=1001:root . /usr/src/app


RUN npm rebuild esbuild

#### generate build --prod
RUN npm run build

### STAGE 2: Run ###
FROM nginxinc/nginx-unprivileged AS runner

#### copy nginx conf
COPY ./nginx/*.conf /etc/nginx/conf.d/

USER 1001

#### copy artifact build from the 'build environment'
COPY --from=build /usr/src/app/ /usr/share/nginx/html

#### don't know what this is, but seems cool and techy
CMD ["nginx", "-g", "daemon off;"]
