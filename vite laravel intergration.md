# Intergration with Vite and Laravel

## Table of contents

- [Backend configuration](#backend-configuration)
  - [Install Laravel](#install-laravel)
  - [Enable Hot Module Replacement](#enable-hot-module-replacement)
- [Frontend configuration](#frontend-configuration)
  - [Option - Install TailwindCSS to the project](#option---install-tailwindcss-to-the-project)
  - [Option - Install Vue3 to the project](#option---install-vue3-to-the-project)
    - [Option - Install Vue router and Pinia](#option---install-vue-router-and-pinia)
  - [Option - Install React to the project](#option---install-react-to-the-project)

## Backend configuration

### Install Laravel
1. Create the new app by ```composer create-project laravel/laravel <YOUR_APP_NAME>```

### Enable Hot Module Replacement

1. Refreshing On Save by set ```refresh``` key to ```true``` and add server object as following example.

```js
const host = ```<YOUR_HOST>```; 
import { defineConfig } from 'vite';
import laravel from 'laravel-vite-plugin';

export default defineConfig({
    plugins: [
        laravel({
            input: ['resources/css/app.css', 'resources/js/app.js'],
            refresh: true
        })
    ],
    resolve: {
        alias: {
            '@': '/resources/js',
        },
    },
    server: {
        host,
        hmr: {
            host,
        },
        watch: {
            usePolling: true
        }
    },
});
```

2. Run ```yarn dev``` and access ```http://<YOUR_HOST_NAME_IN_ENV_FILE>``` on Web browser.


## Frontend configuration

1. Install several dev plugins including ```npm install``` or ```yarn install```

2. Overwrite the ```@``` alias by adding your own to the vite.config.js

Example:
```js
import { defineConfig } from 'vite';
import laravel from 'laravel-vite-plugin';
 
export default defineConfig({
    plugins: [
        laravel({
          input: ['resources/css/app.css', 'resources/js/app.js'],
          refresh: true,
        }),
    ],
    resolve: {
        alias: {
          '@': '/resources/js',
        },
    }
});
```

3. Create ```app.blade.php```, add ```@vite('resources/css/app.css')``` under the style tag to loading your styles and ```@vite('resources/js/app.js'])``` under the script tag to loading your script.

4. Open ```routes/web.php``` and replace ```welcome.blade.php``` with ```app.blade.php``` file to load the ```app.blade.php``` file where our javascript code will execute.

```php
// web.php
Route::get('/', function () {
    return view('app');
});
```

5. Run ```yarn build``` for production.

### Option - Install TailwindCSS to the project

1. 

```bash
yarn add tailwindcss autoprefixer postcss
yarn add -D sass
```

2. Create the tailwind.config.js by `tailwind init`

3. Replace the contents of ```tailwind.config.js``` with the following, if you want to install Vue, the extension ```vue``` should be added in the content object and for Typescript, ```ts``` should be added.

```js

/** @type {import('tailwindcss').Config} */
const colors = require('tailwindcss/colors')
const defaultTheme = require('tailwindcss/defaultTheme')

module.exports = {
  content: [
    'resources/js/**/*.{ts,js,vue}'
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

4. Replace the contents of ```resources/css/app.css``` with the following:

```css
@import "tailwindcss/base";
@import "tailwindcss/components";
@import "tailwindcss/utilities";
```


5. Create a file named ```postcss.config.cjs``` in the project root and add the following:

```js
module.exports = {
  plugins: {
    tailwindcss: {},
    autoprefixer: {},
  },
}
```

### Option - Install Vue3 to the project

1. ```yarn add -D @vitejs/plugin-vue @vue/compiler-sfc```

2. ```yarn add vue@latest``` for latest version of Vue3 and ```yarn add vue@next``` for stable version

3. In ```vite.config.js```, import ```@vitejs/plugin-vue``` and add ```vue()``` in plugins object.

Example:
```js
// vite.config.js
import { defineConfig } from 'vite';
import laravel from 'laravel-vite-plugin';
import vue from '@vitejs/plugin-vue'

export default defineConfig({
    plugins: [
        vue(),
        laravel([
            'resources/css/app.css',
            'resources/js/app.js',
        ]),
    ],
});
```
4. Create a component for testing.
```js
// /resources/js/app.js
import {createApp} from 'vue'

import App from './App.vue'

createApp(App).mount("#app")
```
and
```vue
<template>
  <div class="mx-auto">
    <h1 id="title" class="text-center">{{ title }}</h1>
  </div>
</template>
<script setup>
import { ref } from 'vue'
const title = ref('Hello!')
</script>
<style scoped>
#title {
  color: wheat;
}
</style>
```
5. Run ```yarn build``` for production.

### Option - Install Typescript to the project

1. 
```bash
yarn add -D typescript @types/node ts-node prettier @vue/eslint-config-prettier @vue/eslint-config-typescript @vue/tsconfig @tsconfig/node18 dotenv-expand
```

2. Change the extension of javescript file to typescript.

e.g. From ```resources/js/app.js``` to ```resources/js/app.ts``` and From ```vite.config.js``` to ```vite.config.ts```


3. Alias configurations for typescript and .vue files

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

4. Add type signature to ```App.vue```

```vue
<template>
  <div class="mx-auto">
    <h1 id="title" class="text-center">{{ title }}</h1>
  </div>
</template>
<script setup lang="ts">
import { ref } from 'vue'
const title = ref<string>('Hello!')
</script>
<style scoped>
#title {
  color: wheat;
}
</style>
```


#### Option - Install Vue router and Pinia

TBC

### Option - Install React to the project

TBC
