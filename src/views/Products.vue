<template>
  <div class="app">
    <div class="container">
      <div class="row py-3">
        <div class="col-md">
          <h2>產品列表</h2>
          <div class="text-end">
            <button
              type="button"
              class="btn btn-outline-primary"
              style="margin-bottom: -20px"
              @click="openModal('add')"
            >
              建立新的產品
            </button>
          </div>
          <table class="table table-hover mt-4">
            <caption>
              此頁面
              {{
                products.length
              }}
              項產品
            </caption>
            <thead>
              <tr>
                <th scope="col">產品名稱</th>
                <th scope="col">原價</th>
                <th scope="col" @click="ascending = !ascending">
                  售價
                  <i class="fas fa-sort"></i>
                </th>
                <th scope="col">是否啟用</th>
                <th scope="col">查看細節</th>
                <th scope="col">編輯</th>
              </tr>
            </thead>
            <tbody>
              <tr v-for="(item, index) in sortProducts" :key="index">
                <td>{{ item.title }}</td>
                <td>{{ item.origin_price }}</td>
                <td>{{ item.price }}</td>
                <td>
                  <span v-if="item.is_enabled" class="text-success">啟用</span>
                  <span v-else>停用</span>
                </td>
                <td>
                  <button
                    type="button"
                    class="btn btn-outline-primary"
                    @click="showDetailModal(item)"
                  >
                    查看細節
                  </button>
                </td>
                <td>
                  <div class="btn-group">
                    <button
                      type="button"
                      class="btn btn-outline-secondary"
                      @click="openModal('edit', item)"
                    >
                      修改
                    </button>
                    <button
                      type="button"
                      class="btn btn-outline-danger"
                      @click="openModal('delete', item)"
                    >
                      刪除
                    </button>
                  </div>
                </td>
              </tr>
            </tbody>
          </table>
          <Pagination :pages="pagination" @get-product="getProducts" />
        </div>
      </div>
    </div>
    <!-- DetailModal -->
    <DetailModal :temp-product="tempProduct" />
    <!--end -->
    <!-- DleteModal head -->
    <DeleteModal :productId="productId" @get-products="getProducts" />
    <!-- DleteModal end -->

    <!-- UpdateOrAddModal head -->
    <AddEditModal
      :edit-or-add-product="editOrAddProduct"
      :product-id="productId"
      :add-product="addProduct"
      @get-products="getProducts"
    />
    <!-- UpdateOrAddModal end -->
  </div>
</template>

<script>
import AddEditModal from "@/components/AddEditModal.vue";
import DeleteModal from "@/components/DeleteModal.vue";
import Pagination from "@/components/Pagination.vue";
import DetailModal from "@/components/ProductDetail.vue";
import { Modal } from "bootstrap";
export default {
  name: "ProductList",
  data() {
    return {
      url: "https://vue3-course-api.hexschool.io/v2", // 請加入站點
      path: "hexschooljerry", // 請加入個人 API Path
      products: [],
      tempProduct: {},
      editOrAddProduct: {
        imagesUrl: [],
      },
      productId: "",
      sortBy: "price",
      ascending: true,
      deleteModal: null,
      productModal: null,
      detailModal: null,
      addProduct: false,
      pagination: {},
    };
  },
  components: {
    AddEditModal,
    DeleteModal,
    Pagination,
    DetailModal,
  },
  methods: {
    checkLogin() {
      // #3 取得 Token（Token 僅需要設定一次）
      /* eslint-disable */
      const token = document.cookie.replace(
        /(?:(?:^|.*;\s*)hexToken\s*\=\s*([^;]*).*$)|^.*$/,
        "$1"
      );
      this.axios.defaults.headers.common["Authorization"] = token;
      // #4  確認是否登入
      this.axios
        .post(`${this.url}/api/user/check`)
        .then((res) => {
          if (!res.data.success) {
            this.$router.push("/");
          }
          this.getProducts();
        })
        .catch((error) => {
          alert(error.response.data.message);
          this.$router.push("/");
        });
    },
    showDetailModal(item) {
      this.tempProduct = item;
      this.detailModal.show();
    },
    getProducts(page = 1) {
      this.productModal.hide();
      this.deleteModal.hide();
      this.axios
        .get(`${this.url}/api/${this.path}/admin/products/?page=${page}`)
        .then((res) => {
          this.products = Object.values(res.data.products).map((item) => item);
          this.pagination = res.data.pagination;
        })
        .catch((error) => {
          alert(error.response.data.message);
        });
    },
    openModal(control, item) {
      //console.log(Array.isArray(this.editOrAddProduct.imagesUrl)); // Array.isArray 若為空陣列則會為false
      if (control == "add") {
        this.addProduct = true;
        this.editOrAddProduct = {
          imagesUrl: [],
        };
        this.productModal.show();
      } else if (control == "edit") {
        this.productId = item.id;
        this.editOrAddProduct = { ...item };
        this.addProduct = false;
        console.log(this.productModal, "outside");
        this.productModal.show();
      } else if (control == "delete") {
        this.productId = item.id;
        this.deleteModal.show();
      }
    },
  },
  mounted() {
    //在mounted才拿抓到DOM元素 這樣就可以在Modal使用 Data的資料
    this.deleteModal = new Modal(document.querySelector("#deleteModal"), {
      keyboard: false, //可以透過esc關閉modal false則是取消此功能  預設為true
    });
    this.productModal = new Modal(document.querySelector("#productModal"), {
      keyboard: false,
    });
    this.detailModal = new Modal(document.querySelector("#detailModal"), {
      keyboard: false,
    });

    this.checkLogin();
  },
  computed: {
    sortProducts() {
      const listSort = this.products.sort((a, b) => {
        return this.ascending ? a.price - b.price : b.price - a.price;
      });
      return listSort;
    },
  },
};
</script>
