# Vue work shop building app

src : [link source](https://jayway.github.io/vue-js-workshop/docs)

## Chap 1. Create you first Vue.js project

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8">
    <title>Hello Vue</title>
    <script src="https://unpkg.com/vue"></script>
    <script type="text/javascript">
      document.addEventListener('DOMContentLoaded',function9){
        new Vue({
          el: '#app',
          data: {
            message: 'Hello Vue.'
          }
        });
      });
    </script>
  </head>
  <body>
      <div id="app">
        {{message}}
      </div>
  </body>
</html>
```

## Chap 2 Using vue-cli for quick project scaffolding

```sh
$ npm install -g vue-cli
```
create vue project
```sh
$ vue init <template> <name>
```

## Chap 3 List products

> create the ProductList component

```sh
$ touch src/component/ProductList.vue
```

> ProductList.vue

```html
<template>
  <table class="table table-hover product-table">
    <thead>
      <tr>
        <th>Name</th>
        <th>Description</th>
        <th>Price</th>
      </tr>
    </thead>
    <tbody>
      <tr v-for="product in products" track-by="id">
        <td>{{product.name}}</td>
        <td>{{product.description}}</td>
        <td>{{product.price}}:-</td>
      </tr>
    </tbody>
  </table>
</template>

<script>
export default {
  data () {
    return {
      products: [
        {
          id: 'cc919e21-ae5b-5e1f-d023-c40ee669520c',
          name: 'COBOL 101 vintage',
          description: 'Learn COBOL with this vintage programming book',
          price: 399,
        },
        {
          id: 'bcd755a6-9a19-94e1-0a5d-426c0303454f',
          name: 'Sharp C2719 curved TV',
          description: 'Watch TV like never before with the brand new curved ' +
            'screen technology',
          price: 1995,
        },
        {
          id: '727026b7-7f2f-c5a0-ace9-cc227e686b8e',
          name: 'Remmington X mechanical keyboard',
          description: 'Excellent for gaming and typing, this Remmington X ' +
            'keyboard features tactile, clicky switches for speed and accuracy',
          price: 595,
        }
      ]
    }
  }
}
</script>
```

> App.vue

```html
<template>
  <product-list></product-list>
</template>

<script>
import ProductList from './components/ProductList';

export default {
  components: {
    ProductList
  }
}
</script>
```

### 3-1 Add ManageProducts

> create ManageProducts.vue

```html
<template>
  <product-list></product-list>
</template>

<script>
import ProductList from './ProductList';

export default {
  components: {
    ProductList
  }
}
</script>
```

> App.vue

```html
<template>
  <manage-products></manage-products>
</template>

<script>
import ManageProducts from './components/ManageProducts';

export default {
  components: {
    ManageProducts
  }
}
</script>
```

## Chap 4 List ProductList
