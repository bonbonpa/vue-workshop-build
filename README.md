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

## Chap 4 Add Products

> create SaveProductForm.vue

```html
<template lang="html">
  <form>
    <div class="form-group">
      <label for="productName">Product name</label>
      <input type="text" v-model="product.name" name="form-control" id="productName" maxlength="32" placeholder="Enter product name">
    </div>
    <div class="form-group">
      <label for="productDescription">Product description <small class="text-muted">(optional)</small></label>
      <textarea class="form-control" v-model="product.description" id="productDescription" rows="3" maxlength="128" palceholder="Enter description">
      </textarea>
    </div>
    <div class="form-group">
      <label for="price">Price</label>
      <input type="number" v-model="product.price" class="form-control" id="price" placeholder="Enter Price" number>
    </div>
    <button type="submit" v-on:click.prevent="onSubmit" class="btn btnp-primary" name="button">Save Product</button>
  </form>
</template>

<script>
export default {
  props: ['product'],
  methods: {
    onSubmit() {
      this.$emit('submit',this.product);
    }
  }
}
</script>

<style lang="css">
</style>

```

> go update ManageProducts.vue

```html
<template>
  <section>
    <save-product-form :product="productInForm"
    v-on:submit="onFormSave">
    </save-product-form>
    <product-list></product-list>
  </section>
</template>

<script>
import ProductList from './ProductList';
import SaveProductForm from './SaveProductForm'

const initialData = () => {
  return {
    productInForm: {
      id: null,
      name: '',
      description: '',
      price: null
    }
  }
}

export default {
  components: {
    ProductList,
    SaveProductForm
  },
  data: initialData,
  methods: {
    onFormSave(productData) {
      console.log('productData', JSON.stringify(productData));
    }
  }
}
</script>
```

> ProductList.vue

```html
<template>
  ...
</template>

<script>
export default {
  props: ['products']
}
</script>
```

> edit SaveProductForm.vue for validate input

```html
<template lang="html">
  <form>
    <div class="form-group" v-bind:class="[{'has-danger': formErrors.name}]">
      <label for="productName">Product name</label>
      <input type="text" v-model="product.name" class="form-control" id="productName" maxlength="32" placeholder="Enter product name">
      <div vi-if="formErrors.name" class="form-control=feedback">{{formErrors.name}}</div>
    </div>
    <div class="form-group">
      <label for="productDescription">Product description <small class="text-muted">(optional)</small></label>
      <textarea class="form-control" v-model="product.description" id="productDescription" rows="3" maxlength="128" placeholder="Enter description">
      </textarea>
    </div>
    <div class="form-group" v-bind:class="[{'has-danger': formErrors.price }]">
      <label for="price">Price</label>
      <input type="number" v-model="product.price" class="form-control" id="price" placeholder="Enter Price" number>
      <div v-if="formErrors.price" class="form-control-feedback">{{formErrors.price}}</div>
    </div>
    <button type="submit" v-on:click.prevent="onSubmit" class="btn btn-primary" >
    Save Product</button>
  </form>
</template>

<script>
export default {
  props: ['product'],
  data () {
    return {
      formErrors: {}
    }
  },
  created () {
    this.$watch('proudct.id' , () => {
      this.formErrors = {};
    })
  },
  methods: {
    validateForm () {
      const errors = {};

      if(!this.product.name){
        errors.name = 'Name is required'
      }
      if(!this.product.price){
        errors.price = 'Price is required'
      }

      this.formErrors = errors;

      return Object.keys(errors).length === 0 ;
    },
    onSubmit () {
      if(this.validateForm())
      {
        this.$emit('submit',this.product);
      }
    },
    onCancel () {
      this.formErrors = {};
      this.$emit('cancel');
    }
  }
}
</script>

<style lang="css">
  form{
    margin-bottom: 24px;
  }
</style>

```

## Chap 5 Update Product

> ManageProducts.vue

```html
<template>
  <section>
    <save-product-form :product="productInForm"
    v-on:submit="onFormSave"
    v-on:cancel="resetProductInForm">
    </save-product-form>
    <product-list :products="products"
    v-on:edit="onEditClicked"
    ></product-list>
  </section>
</template>

<script>
import uuid from 'uuid'
import ProductList from './ProductList';
import SaveProductForm from './SaveProductForm'

const initialData = () => {
  return {
    productInForm: {
      id: null,
      name: '',
      description: '',
      price: null
    },
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
          description: 'Watch TV like never before with the brand new curved screen technology',
          price: 1995,
        },
        {
          id: '727026b7-7f2f-c5a0-ace9-cc227e686b8e',
          name: 'Remmington X mechanical keyboard',
          description: 'Excellent for gaming and typing, this Remmington X keyboard ' +
            'features tactile, clicky switches for speed and accuracy',
          price: 595,
        }
      ]
  }
}

export default {
  components: {
    ProductList,
    SaveProductForm
  },
  data: initialData,
  methods: {
    onFormSave(product) {
      //product.id = uuid.v4();
      //this.products.push(product);
      //this.resetProductInForm();
      //console.log('productData', JSON.stringify(productData));

      const index = this.products.findIndex((p) => p.id === product.id);

      // update product if exists or create it id it doesn't
      if(index !== -1){
        //
        this.products.splice(index, 1, product);
      }else{
        product.id = uuid.v4();
        this.products.push(product);
      }
      this.resetProductInForm();
    },
    resetProductInForm() {
      this.productInForm = initialData().productInForm;
    },
    onEditClicked(product) {
      this.productInForm = { ...product };
    }
  }
}
</script>

```

> SaveProductForm.vue

```html
<template lang="html">
  <form>
    <div class="form-group" v-bind:class="[{'has-danger': formErrors.name}]">
      <label for="productName">Product name</label>
      <input type="text" v-model="product.name" class="form-control" id="productName" maxlength="32" placeholder="Enter product name">
      <div vi-if="formErrors.name" class="form-control=feedback">{{formErrors.name}}</div>
    </div>
    <div class="form-group">
      <label for="productDescription">Product description <small class="text-muted">(optional)</small></label>
      <textarea class="form-control" v-model="product.description" id="productDescription" rows="3" maxlength="128" placeholder="Enter description">
      </textarea>
    </div>
    <div class="form-group" v-bind:class="[{'has-danger': formErrors.price }]">
      <label for="price">Price</label>
      <input type="number" v-model="product.price" class="form-control" id="price" placeholder="Enter Price" number>
      <div v-if="formErrors.price" class="form-control-feedback">{{formErrors.price}}</div>
    </div>
    <button type="submit" v-on:click.prevent="onSubmit" class="btn btn-primary" >
    {{ product.id ? 'Update' : 'Add' }} Product</button>
    <button type="submit" v-if="product.id" class="btn btn-secondary" v-on:click.prevent="onCancel">Cancel</button>
  </form>
</template>

<script>
export default {
  props: ['product'],
  data () {
    return {
      formErrors: {}
    }
  },
  created () {
    this.$watch('proudct.id' , () => {
      this.formErrors = {};
    })
  },
  methods: {
    validateForm () {
      const errors = {};

      if(!this.product.name){
        errors.name = 'Name is required'
      }
      if(!this.product.price){
        errors.price = 'Price is required'
      }

      this.formErrors = errors;

      return Object.keys(errors).length === 0 ;
    },
    onSubmit () {
      if(this.validateForm())
      {
        this.$emit('submit',this.product);
      }
    },
    onCancel () {
      this.formErrors = {};
      this.$emit('cancel');
    }
  }
}
</script>

<style lang="css">
  form{
    margin-bottom: 24px;
  }
</style>

```

## Chap 6 Remove products

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
      <tr v-for="product in products" track-by="id" v-on:click.prevent="onEdit(product)">
        <td>{{product.name}}</td>
        <td>{{product.description}}</td>
        <td>{{product.price}}:-</td>
        <td><a href="#" v-on:click.prevent.stop="onRemove(product)">remove</a></td>
      </tr>
      <tr v-if="!products.length">
        <td colspan="4" class="p-y-3 text-xs-center">
          <strong> You should add some products!</strong>
        </td>
      </tr>
    </tbody>
  </table>
</template>

<script>
export default {
  props: ['products'],
 methods: {
   onEdit(product) {
     this.$emit('edit', product)
   },
   onRemove(product) {
     this.$emit('remove',product)
   }
 }
}
</script>
```

> MangeProduct.vue

```html
<template>
  <section>
    <save-product-form :product="productInForm"
    v-on:submit="onFormSave"
    v-on:cancel="resetProductInForm"
    >
    </save-product-form>
    <product-list :products="products"
    v-on:edit="onEditClicked"
    v-on:remove="onRemoveClicked"
    ></product-list>
  </section>
</template>

<script>
import uuid from 'uuid'
import ProductList from './ProductList';
import SaveProductForm from './SaveProductForm'

const initialData = () => {
  return {
    productInForm: {
      id: null,
      name: '',
      description: '',
      price: null
    },
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
          description: 'Watch TV like never before with the brand new curved screen technology',
          price: 1995,
        },
        {
          id: '727026b7-7f2f-c5a0-ace9-cc227e686b8e',
          name: 'Remmington X mechanical keyboard',
          description: 'Excellent for gaming and typing, this Remmington X keyboard ' +
            'features tactile, clicky switches for speed and accuracy',
          price: 595,
        }
      ]
  }
}

export default {
  components: {
    ProductList,
    SaveProductForm
  },
  data: initialData,
  methods: {
    onFormSave(product) {
      //product.id = uuid.v4();
      //this.products.push(product);
      //this.resetProductInForm();
      //console.log('productData', JSON.stringify(productData));

      const index = this.products.findIndex((p) => p.id === product.id);

      // update product if exists or create it id it doesn't
      if(index !== -1){
        //
        this.products.splice(index, 1, product);
      }else{
        product.id = uuid.v4();
        this.products.push(product);
      }
      this.resetProductInForm();
    },
    resetProductInForm() {
      this.productInForm = initialData().productInForm;
    },
    onEditClicked(product) {
      this.productInForm = { ...product };
    },
    onRemoveClicked(product) {
      const index = this.products.findIndex((p) => p.id === product.id);

      this.products.splice(index,1);

      if(product.id === this.productInForm.id){
        this.resetProductInForm();
      }
    }
  }
}
</script>
```

## Chap 7 Routing with vue-router

> install vou-router

```sh
$ npm install --save vue-router
```
> create AppNav.vue

```html
<template>
    <nav class="navbar navbar-full navbar-light navbar-jw">
        <a class="navbar-brand" href="#" >
            <img src="../assets/jay.svg" alt="Jayway logo" />bonbonpa
        </a>
        <div class="nav navbar-nav">
            <router-link 
            to="home" 
            class="nav-item nav-link" 
            active-class="active" 
            exact>
                Home
            </router-link>
            <router-link 
            to="manage-products" 
            class="nav-item nav-link" 
            active-class="active" 
            exact>
                Manage Products
            </router-link>
        </div>
    </nav>
</template>

<script>
    export default {

    }
</script>
```

> Create ProductCatalog.vue

```html
<template>
    <p>I'm a product catalog!</p>
</template>

<script>
    export default{
        
    }
</script>
```

> edit App.vue

```html
<template>
  <div>
    <header class="app-header">
      <app-nav></app-nav>
    </header>
    <main class="container">
      <router-view></router-view>
    </main>
  </div>
</template>

<script>
import AppNav from './components/AppNav'

export default {
  components: {
    AppNav
  }
}
</script>

```

> edit main.js

```js
import Vue from 'vue'
import App from './App'
import VueRouter from 'vue-router'

import ProductCatalog from './components/ProductCatalog'
import ManageProducts from './components/ManageProducts'


import './styles/style.scss'

Vue.use(VueRouter)

const routes = [
    { path: '/home', alias: '/', component: ProductCatalog },
    { path: '/manage-products', component: ManageProducts }
]

const router = new VueRouter({
    routes
})

new Vue({
    el: '#app',
    router,
    render: h => h(App)
})
```
