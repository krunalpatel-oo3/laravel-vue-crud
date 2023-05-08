<template>
  <div>
    <h2 v-if="isNewProduct">Add Product</h2>
    <h2 v-else>Edit Product</h2>
      <form @submit.prevent="submitForm">
        <div class="mb-3">
          <label for="name" class="form-label">Name:</label>
          <input class="form-control" type="text" id="name" v-model="product.name" required />
        </div>
        <div class="mb-3">
          <label for="description" class="form-label">Description:</label>
          <textarea class="form-control" id="description" v-model="product.description"></textarea>
        </div>
        <div class="mb-3">
          <label for="price" class="form-label">Price:</label>
          <input class="form-control" min="0" type="number" id="price" v-model="product.price"  />
        </div>
        <div class="mb-3">
          <label for="price" class="form-label">Is home?: </label>
          <input v-model="product.is_home" type="radio" :value="1" name="is_home" />
          <label class="ml-5" for="1">Yes</label>
          <input v-model="product.is_home" type="radio" :value="0" name="is_home" checked />
          <label class="ml-5" for="0">No</label>
        </div>
        <div class="mb-3">
          <label class="form-label">Colors: </label>&nbsp;
          <input type="checkbox" v-model="colors_input" name="colors[]" :value="red" id="red_color" />
          <label class="form-check-label" for="red_color">Red</label>&nbsp;
          <input type="checkbox" v-model="colors_input" :value="blue" name="colors[]" id="blue_color"/>
          <label class="form-check-label" for="blue_color">Blue</label>&nbsp;
          <input type="checkbox" v-model="colors_input" :value="green" name="colors[]" id="green_color"/>
          <label class="form-check-label" for="green_color">Green</label>
        </div>
        <button type="submit" v-if="isNewProduct" class="btn btn-primary">Add Product</button>
        <button type="submit" v-else class="btn btn-primary">Update Product</button>
      </form>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  data() {
    return {
      product: {
        name: '',
        description: '',
        price: 0,
        is_home: '',
        colors_input: []
      }
    }
  },
  computed: {
    isNewProduct() {
      return !this.$route.path.includes('edit');
    }
  },
  async created() {
    if (!this.isNewProduct) {
      const response = await axios.get(`/api/products/${this.$route.params.id}`);
      this.product = response.data;
    }
  },
  methods: {
    async submitForm() {
      try {
        if (this.isNewProduct) {
          await axios.post('/api/products', this.product);
        } else {
          await axios.put(`/api/products/${this.$route.params.id}`, this.product);
        }
        this.$router.push('/');
      } catch (error) {
        console.error(error);
      }
    }
  }
}
</script>