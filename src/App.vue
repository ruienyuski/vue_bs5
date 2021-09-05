<template>

  <div class="text-end">
    <button type="button" class="btn btn-primary text-end" @click="addItem">新增</button>
  </div>

  <table class="table">
    <thead>
      <tr>
        <th>標題</th>
        <th>圖片</th>
        <th>銷售狀態</th>
        <th>編輯</th>
      </tr>
    </thead>
    <tbody>
      <tr v-for="item in products" :key="item.id"
        :class="{'table-success': item.onStock}">
        <td>{{ item.name }}</td>
        <td>
          <img :src="item.imageUrl" alt="" height="100">
        </td>
        <td>
          <input type="checkbox" v-model="item.onStock">
        </td>
        <td>
          <button type="button" class="btn btn-outline-primary"
         @click="editItem(item)">修改</button>
        <!-- Button trigger modal -->
        <button type="button" 
        class="btn btn-danger" 
        data-bs-toggle="modal" 
        data-bs-target="#removeModal"
        @click="removeModal(item)"
        >
          刪除
        </button>      
        </td>
      </tr>
    </tbody>
  </table>
  <form v-if="status">
    <div class="mb-3">
      <label for="productName" class="form-label">產品名稱</label>
      <input type="text" id="productName"
        class="form-control"
        v-model="temp.name"
      >
    </div>
    <div class="mb-3">
      <label for="productImage"
      class="form-label">產品圖片</label>
      <input type="text" id="productImage"
        class="form-control"
        v-model="temp.imageUrl"
      >
    </div>
    <button type="button"
    class="btn btn-secondary"
    @click="confirmEdit">更新</button>
  </form>
<!-- Modal -->
<div class="modal fade" id="removeModal" ref="remove" tabindex="-1" aria-labelledby="removeModalLabel" aria-hidden="true">
  <div class="modal-dialog">
    <div class="modal-content">
      <div class="modal-header">
        <h5 class="modal-title" id="removeModalLabel">刪除資料</h5>
        <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
      </div>
      <div class="modal-body">
        請問要刪除 {{temp.name}} 嗎?
      </div>
      <div class="modal-footer">
        <button type="button" class="btn btn-secondary" data-bs-dismiss="modal">取消</button>
        <button type="button" class="btn btn-danger" @click="removeItem">確認刪除</button>
      </div>
    </div>
  </div>
</div>  

</template>

<script>
/* global bootstrap */

const products = [{
  id: '1',
  imageUrl: 'https://images.unsplash.com/photo-1516906571665-49af58989c4e?ixlib=rb-1.2.1&ixid=MXwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHw%3D&auto=format&fit=crop&w=300&q=80',
  name: 'MacBook Pro',
  onStock: false,
},{
  id: '2',
  imageUrl: 'https://images.unsplash.com/photo-1512499617640-c74ae3a79d37?ixid=MXwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHw%3D&ixlib=rb-1.2.1&auto=format&fit=crop&w=300&q=80',
  name: 'iPhone',
  onStock: false,
}];
export default {
  name: 'App',

  data() {
    return {
      products: [],
      temp: {},
      status:false,
      modal: {},
      collapse: {},
    }
  },
  methods: {
    addItem() {
      this.status = true;
      this.temp = {};
    },
    removeModal(item) {
      this.temp = item
      this.modal.show()      
    },
    removeItem() {
      this.products.forEach((el,i)=>{
        if(el.id === this.temp.id) {
          this.products.splice(i,1)
        }
      })
      this.modal.hide()
    },
    editItem(item) {
      this.status = true;
      this.temp = { ...item }
    },
    confirmEdit() {
      if (!this.temp.id) {
        this.temp.id = new Date().getTime();
        this.temp.onStock = false;
        this.products.push(this.temp);
        this.isNew = false;
      } else {
        this.products.forEach((item, i) => {
          if (item.id === this.temp.id) {
            this.products[i] = this.temp;
          }
        });
      }
      this.temp = {};
      this.status = false;
    },
  },
  created() {
    this.products = products;
  },  
  mounted() {
    this.modal = new bootstrap.Modal(this.$refs.remove)
  }
}
</script>

<style lang="scss">
@import '~bootstrap/scss/bootstrap';
/* #app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
} */
</style>
