# sample-tinymce-vue-selfhosted

The goal of this project is to serve as a template for projects using a sel-hosted version of TinyMCE while loading local premium plugins.

There are multiple ways of loading premium plugins into a self-hosted dpeloyment. This particular example relies on copying the contents of the plugin into the TinyMCE plugins folder and using that enhanced instance of TinyMCE in our components. We want to avoid the `tinymce-vue` wrapper creating a new instance from the cloud so we generate the enhanced instance on the application `index.html` file to make available for our components.

## Bundling the self-hosted configuration

1. First install TinyMCE and the `tinymce-vue` wrapper from npm

```
yarn add tinymce
yarn add @tinymce/tinymce-vue
```

2. We need to allow access to the TinyMCE code. We can do this by copying the Tinymce folder from our `node_modules` folder into our `public` folder. You can also use a webpack plugin to copy the files where you need them to be.

```
rsync -a node_modules/tinymce public/
```

3. We need to enhance our TinyMCE instance with our premium plugins. In this case we are using a single plugin ('fake_plugin') but you can copy any number of external or premium plugins here.

```
rsync -a fake_plugin public/tinymce/plugins/
```

4. To ensure that our TinyMCE is always the latest version we can do this before building or serving (lines 5 to 10 in `package.json`).

```
"scripts": {
  "tiny": "rsync -a node_modules/tinymce public/ ; rsync -a fake_plugin public/tinymce/plugins/",
  "serve": "yarn tiny && vue-cli-service serve",
  "build": "yarn tiny && vue-cli-service build",
  "lint": "vue-cli-service lint"
},
```

5. Next, we will load the TinyMCE script in our application by adding a script reference in our `index.html` file (line 9 in `index.html`).

```
<script src="./tinymce/tinymce.min.js"></script>
```

6. Finally, we can use the editor in our Vue components by importing the Editor component from `@tinymce-tinymce-vue` (lines 9 to 12, 17, and 22 in `src/components/HelloWorld.vue`)

```
<editor :init="{
  plugins: 'fake_plugin help',
  toolbar: 'undo redo | fake | help'
}"></editor>
<script>
import Editor from '@tinymce/tinymce-vue';

export default {
  name: 'HelloWorld',
  components: {
    'editor': Editor
  },
  props: {
    msg: String
  }
}
</script>
```

## Project setup
```
yarn install
```

### Compiles and hot-reloads for development
```
yarn serve
```

### Compiles and minifies for production
```
yarn build
```

### Lints and fixes files
```
yarn lint
```

### Customize configuration
See [Configuration Reference](https://cli.vuejs.org/config/).
