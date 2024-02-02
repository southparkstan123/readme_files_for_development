# Intergration with Vite and Ruby on rails

## Table of contents

- [Backend configuration](#backend-configuration)
  - [Create Rails project](#create-rails-project)
  - [Environment variables and database configurations](#environment-variables-and-database-configurations)
- [Frontend cofiguration](#frontend-cofiguration)
  - [Option - Install TailwindCSS to the project](#option---install-tailwindcss-to-the-project)
  - [Option - Install Vue3 to the project](#option---install-vue3-to-the-project)
    - [Option - Install Vue router and Pinia](#option---install-vue-router-and-pinia)
  - [Option - Install React to the project](#option---install-react-to-the-project)

## Backend configuration

### Create Rails project

1. Create the new app by ```rails new <APP_NAME> -d <DATABASE_TYPE>```

2. Add the ```gem "vite_rails"``` and ```gem "vite_rails"``` to the ```Gemfile```. Of course you can more packages add on it.

Example:
```ruby
# ...
# Use vite_rails and vite_ruby as the frontend tooling
gem "vite_rails"
gem "vite_ruby"
# Bundle edge Rails instead: gem 'rails', github: 'rails/rails'
gem 'rails', '7.0.0'
# Necessary for upgrage to Rail 7
gem "sprockets-rails"
# Use mysql as the database for Active Record
gem 'mysql2', '~> 0.5' #Remark: Modify it if you want to use another database such as sqlite 
# Use Puma as the app server
gem 'puma', '~> 5.0'
# Use SCSS for stylesheets
gem 'sass-rails', '>= 6'
# Transpile app-like JavaScript. Read more: https://github.com/rails/webpacker
gem 'webpacker', '~> 5.0'
# Turbolinks makes navigating your web application faster. Read more: https://github.com/turbolinks/turbolinks
gem 'turbolinks', '~> 5'
# Build JSON APIs with ease. Read more: https://github.com/rails/jbuilder
gem 'jbuilder', '~> 2.7'
# Use Redis adapter to run Action Cable in production
gem 'redis', '~> 4.0'
# Use Active Model has_secure_password
gem 'bcrypt', '~> 3.1.7'
# Allows the Cross-Origin Resource Sharing (CORS) in the API
gem 'rack-cors'
# JSON Web Token
gem 'jwt'
# XML parse
gem 'rexml'
# Load environment variables from env file
gem 'dotenv-rails', groups: %i[development test]
# ...
```

and


```ruby
group :development do
  # ...
  # Use Foreman as the process manager for applications
  gem 'foreman', '~> 0.87.2'
end
```

3. Run ```bundle```

4. Run ```bundle exec vite install```

### Environment variables and database configurations

1. Add the .env files to store the configuration value for different environments , the example file is in ```.env.template```, just copy this file for specific environment. 

Example of ```.env.template```:

```
JWT_SECRET=JWT_SECRET
DATABASE_USERNAME=DATABASE_USERNAME
DATABASE_PASSWORD=DATABASE_PASSWORD
HOST_NAME=HOST_NAME
DATABASE_NAME=DATABASE_NAME
PORT=PORT
```

For example, in development, create ```.env.development.local```, then input the key and value on it.

2. Add the test configurations for your local machine by copy ```.env.template```, then create and modify as ```.env.test.local```

3. Add in ```/config/environments/development.rb``` and ```/config/environments/test.rb```

```ruby
# To allow requests to designated host
  config.hosts << "#{ENV.fetch("HOST_NAME")}"
```

4. Create the database and migration by following command:
```rails db:create && rails db:migrate```

## Frontend cofiguration

### Option - Install TailwindCSS to the project
1. Install TailwindCSS and several dev plugins including ```vite-plugin-full-reload```

```bash
yarn add tailwindcss autoprefixer postcss

yarn add -D eslint prettier eslint-plugin-prettier eslint-config-prettier eslint-plugin-tailwindcss path vite-plugin-full-reload vite-plugin-stimulus-hmr

```

2. Replace the contents of ```vite.config.js``` or ```vite.config.ts``` with

```js

import { defineConfig } from 'vite'
import FullReload from "vite-plugin-full-reload"
import RubyPlugin from 'vite-plugin-ruby'
import StimulusHMR from 'vite-plugin-stimulus-hmr'

export default defineConfig({
    clearScreen: false,
    plugins: [
      RubyPlugin(), 
      StimulusHMR(), 
      FullReload(["config/routes.rb", "app/views/**/*"], { delay: 300 }),
    ]
  }
)

```

3. Create the tailwind.config.js by `tailwind init`

4. Replace the contents of ```tailwind.config.js``` with the following

```js

/** @type {import('tailwindcss').Config} */
const colors = require('tailwindcss/colors')
const defaultTheme = require('tailwindcss/defaultTheme')

module.exports = {
  content: [
    './app/helpers/**/*.rb',
    './app/assets/stylesheets/**/*.css',
    './app/views/**/*.{html,html.erb,erb}',
    './app/javascript/components/**/*.js',
  ],
  theme: {
    fontFamily: {},
    extend: {
    },
  },
  corePlugins: {
    aspectRatio: false,
  },
  plugins: [],
}

```

5. Replace the contents of ```app/assets/stylesheets/application.css``` with the following:

```css
@import "tailwindcss/base";
@import "tailwindcss/components";
@import "tailwindcss/utilities";
```

6. Create a file named ```application.css``` in ```app/javascript/entrypoints/```

```css
@import "../../assets/stylesheets/application.css";
```

7. Create a file named ```postcss.config.js``` in the project root and add the following:

```js
module.exports = {
  plugins: {
    tailwindcss: {},
    autoprefixer: {},
  },
}
```

8. In app/views/layouts/application.html.erb, change the _tags to the following:

```rb
<%= csrf_meta_tags %>
<%= csp_meta_tag %>

<%= vite_client_tag %>
<%= vite_stylesheet_tag 'application', data: { "turbo-track": "reload" } %>
<%= vite_javascript_tag 'application' %>
```

or

```rb
<%= csrf_meta_tags %>
<%= csp_meta_tag %>

<%= vite_client_tag %>
<%= vite_stylesheet_tag 'application', data: { "turbo-track": "reload" } %>
<%= vite_typescript_tag 'application' %>
```

9. Now we’ll add a home page to test our setup
```bash
rails g controller Home index
```
10. And make this the root path by adding the following to the config/routes.rb.
```bash
root 'home#index'
```
11. Replace the contents of app/views/home/index.html.erb this the following

```html
<div id="app"></div>
```

12. Replace the contents of Procfile with

```
# Procfile

web: rails s -b 0.0.0.0 -p 3000
vite: bin/vite dev --clobber
```

13. Add ```server``` object on ```vite.config.ts``` (*Optional* If you are using `Homestead`)

Example:
```js
import { defineConfig } from 'vite'
import FullReload from "vite-plugin-full-reload"
import RubyPlugin from 'vite-plugin-ruby'
import StimulusHMR from 'vite-plugin-stimulus-hmr'

export default defineConfig({
  clearScreen: false,
  plugins: [
    RubyPlugin(),
    StimulusHMR(),
    FullReload(["config/routes.rb", "app/views/**/*"], { delay: 200 }),
  ],
  server: {
    host: "<IP_ADDRESS_FROM_HOMESTEAD>",
    watch: {
      usePolling: true,
    },
  },
})
```

14. Modify ```host``` and ```https``` object on ```vite.json``` (*Optional* If you are using `Homestead`)

```json
// vite.json
{
  "all": {
    "sourceCodeDir": "app/javascript",
    "watchAdditionalPaths": []
  },
  "development": {
    "autoBuild": true,
    "publicOutputDir": "vite-dev",
    "host": "<IP_ADDRESS_FROM_HOMESTEAD>",
    "port": 3036,
    "https": false
  },
  "test": {
    "autoBuild": true,
    "publicOutputDir": "vite-test",
    "host": "<IP_ADDRESS_FROM_HOMESTEAD>",
    "port": 3037,
    "https": false
  }
}
```

15. Start the web and vite processes by ```foreman start```

16. Access ```http://<YOUR_HOST_NAME_IN_ENV_FILE>:3000``` on Web browser.

17. Check if the printed console output 'Vite ⚡️ Rails' shows, the intergration is completed.

😃😃😃😃😃😃😃😃😃😃😃
😃Happy coding!  😃
😃😃😃😃😃😃😃😃😃😃😃

### Option - Install Vue3 to the project

1. In the root directory of project, run following commands:

For Vue3.2.x
```bash
yarn add vue@next @vitejs/plugin-vue 
yarn add -D @vue/compiler-sfc typescript @types/node ts-node prettier @vue/eslint-config-prettier @vue/eslint-config-typescript @vue/tsconfig @tsconfig/node18 dotenv-expand
```

For Latest Vue
```bash
yarn add vue@next @vitejs/plugin-vue 
yarn add -D @vue/compiler-sfc typescript @types/node ts-node prettier @vue/eslint-config-prettier @vue/eslint-config-typescript @vue/tsconfig @tsconfig/node18 dotenv-expand
```

2. Alias configurations for typescript and .vue files

tsconfig.json and vite.config.ts

```json
// tsconfig.json
{
  "compilerOptions": {
    // ...
    "paths": {
      "@/*": ["<Desire path>"]
    }
  }
  // ...
}

```

```js
// vite.config.ts
import { defineConfig } from 'vite'
import vue from '@vitejs/plugin-vue'
import path from 'path'

// https://vitejs.dev/config/
export default defineConfig({
  // Other necessary configurations
  resolve: {
    alias: {
      '@': path.resolve(__dirname, '<Desire path>')
    }
  }
  // Other necessary configurations
})
```


3. Add the following codes on ```app/javascript/entrypoints/application.js``` or ```app/javascript/entrypoints/application.ts``` 

```vue
<template>
  <div>
    <h1>{{ title }}</h1>
  </div>
</template>
<script setup>
// /app/javascript/entrypoints/components/App.vue
import { ref } from 'vue'
const title = ref('Hello!')
</script>
<style scoped>
#title {
  color: wheat;
}
</style>
```

```js
// /app/javascript/entrypoints/application.js
import { createApp, defineComponent } from 'vue'
import App from './components/App.vue'

document.addEventListener('DOMContentLoaded', () => {
  const app = createApp(App)
  app.mount('#app')
})

```

4. Start the web and vite processes by ```foreman start```

5. Access ```http://<YOUR_HOST_NAME_IN_ENV_FILE>:3000``` on Web browser and 

6. Check if the styled title "Hello!" shows, the installation is completed. Let's start developing Vue apps with Vite! 

Happy Coding!!!


#### (Optional) Change directory 'entryPoints' to another name 

If you want to name the directory as desire instead ```entrypoints``` by following steps 

1. Modify ```config/vite.json```
```js
// config/vite.json
{
  "all": {
    "sourceCodeDir": "app/javascript",
    "watchAdditionalPaths": [],
    "entrypointsDir": "<YOUR_DESIRE_DIRECTORY_NAME>"
  },
  ...
```

2. Modify ```tailwind.config.js```
```js
// tailwind.config.js
module.exports = {
  content: [
    './app/helpers/**/*.rb',
    './app/assets/stylesheets/**/*.css',
    './app/views/**/*.{html,html.erb,erb}',
    './app/javascript/<YOUR_DESIRE_DIRECTORY_NAME>/**/*.{js,ts,vue}',
  ],
  ...
```

3. (Optional) Alias by ```vite.config.ts```
```js
//vite.config.ts
...
resolve: {
    alias: {
      '@': fileURLToPath(new URL('app/javascript/<YOUR_DESIRE_DIRECTORY_NAME>', import.meta.url))
    }
  },
```

4. Complier of Typescript
```js
// tsconfig.app.json
{
  "extends": "@vue/tsconfig/tsconfig.dom.json",
  "include": ["env.d.ts", "app/javascript/<YOUR_DESIRE_DIRECTORY_NAME>/**/*.{vue,ts,d.ts}"],
  "exclude": ["src/**/__tests__/*"],
  "compilerOptions": {
    "composite": true,
    "baseUrl": ".",
    "paths": {
      "@/*": [".app/javascript/<YOUR_DESIRE_DIRECTORY_NAME>/*"]
    },
    "noImplicitAny": false,
    "resolveJsonModule": true
  }
}
```

5. Change the directory name manaully.
```bash
cd app/javascript
mv /entrypoints /<YOUR_DESIRE_DIRECTORY_NAME>
```

#### Option - Install Vue router and Pinia

TBC

### Option - Install React to the project

TBC
