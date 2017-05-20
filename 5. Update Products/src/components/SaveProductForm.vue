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
