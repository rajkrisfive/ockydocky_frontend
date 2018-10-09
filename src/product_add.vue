<template>
  <div>
    <b-form inline @submit="onSubmit">
      <label class="mr-sm-2" for="productTitle">Title</label>
      <b-input class="mb-2 mr-sm-2 mb-sm-0" id="productTitle" v-model="form.title"
      placeholder="Title" required />
      <label class="mr-sm-2" for="category">Category</label>
      <b-form-select class="mb-2 mr-sm-2 mb-sm-0"
                     :options="categoryOptions"
                     id="category"
                     v-model="form.catSelected"
                     @input="getSubCategory"
                     >
      </b-form-select>

      <label class="mr-sm-2" for="subCategory">SubCategory</label>
      <b-form-select class="mb-2 mr-sm-2 mb-sm-0"
                     :options="subCategoryOptions"
                     id="subCategory"
                     v-model="form.subCatSelected">
      </b-form-select>

      <b-button type="submit" variant="primary">Add</b-button>&nbsp;&nbsp;
      <b-button type="button" variant="danger" @click="cancelAdd">Cancel</b-button>&nbsp;&nbsp
      <span v-show="formStatus">{{formStatusMessage}}</span>
    </b-form>
    </br>
  </div>
</template>
<script>
import axios from 'axios'
export default {
name: 'product-add',
data() {
return {
form: {
        title: '',
        catSelected: null,
        subCatSelected: null
      },
formStatusMessage: '',
formStatus: false,
categoryOptions: [],
subCategoryOptions: [],
categoryOptionsEndPoint: 'http://localhost:8000/products/category-list/',
subCategoryOptionsEndPoint: 'http://localhost:8000/products/sub-category-list/',
productAddEndPoint: 'http://localhost:8000/products/product-list-create/'
}
},
created() {
 this.getCategoryOptions();
},
methods: {
 getCategoryOptions(){
       axios.get(this.categoryOptionsEndPoint)
      .then(response => {
      this.categoryOptions = response.data.map((x) => {return {text: x.title, value: x.id}});
      this.form.catSelected = this.categoryOptions.length ? this.categoryOptions[0].value : null;
      })
      .catch(error => {
      console.log(error);
      })
 },
 getSubCategory(value){
     axios.get(this.subCategoryOptionsEndPoint + "?category=" + value)
    .then(response => {
    this.subCategoryOptions = response.data.map((x) => {return {text: x.title, value: x.id}});
    this.form.subCatSelected = this.subCategoryOptions.length ? this.subCategoryOptions[0].value : null;
    })
    .catch(error => {
    console.log(error);
    })
 },
 addProduct(){
       axios.post(this.productAddEndPoint, {title: this.form.title,
       sub_category: this.form.subCatSelected})
      .then(response => {
      this.formStatusMessage = 'Operation Successfull';
      this.$emit('addSuccessful');
      this.form.title = '';
      })
      .catch(error => {
      this.formStatusMessage = 'Operation not completed.';
      })
      this.formStatus = true;
 },
 cancelAdd(){
 this.formStatus = false;
 this.formStatusMessage = '';
 this.$emit('addCancel');
 },
 onSubmit (evt) {
       evt.preventDefault();
       this.addProduct()
     },
}

}

</script>
<style></style>
