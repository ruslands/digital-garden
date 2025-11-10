vite: is a build tool like Webpack, that will basically allow you to work locally on your app and build it for production. It will also bring to the table various loaders, Hot Module Replacement capabilities, environment variables, assets managements etc.

nuxt: is totally unrelated to all of this. It is comparable to Gatsby/Next/Vitepress for a React/Svelte equivalent.

It's goal is to provide more capabilities to VueJS. For example, Vue can only be run as an SPA, meaning that you will not get any kind of indexing with search engines, while Nuxt do have SSR and SSG modes that will allow your websites to be efficiently crawled.

It also brings a lot of Developer Experience niceties: auto import of the composition API, of the components, simple routing, huge ecosystem thanks to all of Nuxt's modules, simpler configuration etc...

It's more like VueJS apps on steroids basically!

Also, latest version of Nuxt (v3) works with Vite out of the box. So, both of them are complementary because you need a build tool to work nowadays in the JS ecosystem + you get more features than just VueJS.

storybook: Every piece of UI is now a component. The superpower of components is that you don't need to spin up the whole app just to see how they render. You can render a specific variation in isolation by passing in props, mocking data, or faking events.

Storybook is packaged as a small, development-only, workshop that lives alongside your app. It provides an isolated iframe to render components without interference from app business logic and context. That helps you focus development on each variation of a component, even the hard-to-reach edge cases.

localazy: software localization

quasar.dev:

gatsby.js:

vuetify:

pinia:

vuex:

three.js:

anime.js:

мультиязычность i18n:

[https://nuxtjs.org](https://nuxtjs.org)

[https://vuetifyjs.com/en/](https://vuetifyjs.com/en/)

[https://vuestic.dev](https://vuestic.dev)

[https://uno.antfu.me](https://uno.antfu.me)

-Facebook SDK

-OneSignal

-AppsFlyer

-Сохранение куки в WebView

-Отложенные диплинки от Facebook

-Отправка событий с приложения webview в Facebook ads и Google 

-Firebase Messaging

-Firebase SSO

npm: (Node Package Manager) is the default package manager for Node.js and ships pre-installed when you download Node.js on your system

yarn: (Yet Another Resource Navigator) is a relatively new package manager developed by Facebook. It was developed to provide more advanced capabilities that NPM lacked at the time (such as version locking) while also making it safer, more reliable, and more efficient.

nvm: (Node Version Manager). [https://github.com/nvm-sh/nvm](https://github.com/nvm-sh/nvm)

nvm use 16

node -v

npx browserslist@latest --update-db

## yarn

yarn upgrade # update yarn.lock based package.json

yarn install

yarn dev # запустить dev сервер

###SSR###

Проект собирается без Dockerfile

Все настройки через Nginx, указывается путь к каждой странице

server {

listen 8000;

server_name ;

location / {

root /var/www/html;

index index.html;

}

# location = /404.html {

# root /var/www/html;

# }

}

###SPA###

Проект собирается через Dockerfile

# Dockerfile

FROM node:14.17.1-alpine

# create destination directory

RUN mkdir -p /usr/src/nuxt-app

WORKDIR /usr/src/nuxt-app

# update and install dependency

RUN apk update && apk upgrade

RUN apk add git

# copy the app, note .dockerignore

COPY . /usr/src/nuxt-app/

RUN npm install

RUN npm run generate

EXPOSE 3000

ENV NUXT_HOST=0.0.0.0

ENV NUXT_PORT=3000

CMD [ "npm", "start" ]