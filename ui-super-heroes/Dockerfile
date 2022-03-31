
### STAGE 1: Build ###
FROM node:lts-alpine AS build

#### make the 'app' folder the current working directory
WORKDIR /usr/src/app

#### copy both 'package.json' and 'package-lock.json' (if available)
COPY . /usr/src/app

RUN npm rebuild esbuild

#### generate build --prod
RUN npm run build

### STAGE 2: Run ###
FROM nginxinc/nginx-unprivileged AS runner

#### copy nginx conf
COPY ./nginx/*.conf /etc/nginx/conf.d/

#### copy artifact build from the 'build environment'
COPY --from=build /usr/src/app/ /usr/share/nginx/html

#### don't know what this is, but seems cool and techy
CMD ["nginx", "-g", "daemon off;"]
