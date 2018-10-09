<template>
<b-container fluid>
<div class="row">
    <template v-for="variant in ['success']">
        <div class="col-md-4 pb-2" v-for="size in ['lg']" :key="`${variant}_${size}`">
            <b-button style="float:left;" v-show=!addNew :size="size" :variant="variant" @click="changeAddNewStatus">
                Add New
            </b-button>
        </div>
    </template>
</div>
<b-table striped
  stacked="md"
  :current-page="currentPage"
  :per-page="perPage"
  responsive
  hover
  :items="products"
  :fields="fields">
  </b-table>
  <ProductAddComponent v-on:addSuccessful="getProducts" v-on:addCancel="changeAddNewStatus" v-show=addNew changeAddNewStatus=changeAddNewStatus></ProductAddComponent>
  <b-row>
      <b-col md="6" class="my-1">
        <b-pagination :total-rows="totalRows" :per-page="perPage" v-model="currentPage" class="my-0" />
      </b-col>
</b-row>
</b-container>
</template>
<script>
import axios from 'axios'
import ProductAddComponent from './product_add.vue';
export default {
components: {ProductAddComponent},
name: 'product-list',
data () {
      return {
      addNew: false,
      products: [],
      currentPage: 1,
      perPage: 15,
      totalRows: 0,
      fields: [{
          key: 'title',
          sortable: true
        }, {
          key: 'category',
          sortable: true
        },
        {
          key: 'sub_category',
          sortable: true
        }],
      endpoint: 'http://localhost:8000/products/product-list-create/'
      }
},
created() {
 this.getProducts();
},
methods: {
  getProducts() {
            axios.get(this.endpoint)
          .then(response => {
          this.products = response.data;
          this.totalRows = this.products.length;
          })
          .catch(error => {
          console.log(error);
          })
  },
  changeAddNewStatus(){
  this.addNew = this.addNew == true ? false:true;
  }
}

}
</script>
<style>
table {
    font-family: arial, sans-serif;
    border-collapse: collapse;
    width: 100%;
}

td, th {
    border: 1px solid #dddddd;
    text-align: left;
    padding: 8px;
}

tr:nth-child(even) {
    background-color: #dddddd;
}
</style>
