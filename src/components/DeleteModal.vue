<template>
  <!-- DleteModal head -->
  <div
    class="modal fade"
    id="deleteModal"
    data-bs-backdrop="static"
    data-bs-keyboard="false"
    tabindex="-1"
    aria-labelledby="staticBackdropLabel"
    aria-hidden="true"
  >
    <div class="modal-dialog">
      <div class="modal-content">
        <div class="modal-header">
          <h5 class="modal-title" id="deleteModalLabel">刪除產品</h5>
          <button
            type="button"
            class="btn-close"
            data-bs-dismiss="modal"
            aria-label="Close"
          ></button>
        </div>
        <div class="modal-body">請確認是否要刪除</div>
        <div class="modal-footer">
          <button
            type="button"
            class="btn btn-secondary"
            data-bs-dismiss="modal"
          >
            取消
          </button>
          <button type="button" class="btn btn-primary" @click="deleteProduct">
            確認刪除
          </button>
        </div>
      </div>
    </div>
    <!-- <div id="liveAlertPlaceholder"></div> -->
  </div>

  <!-- DleteModal end -->
</template>

<script>
export default {
  data() {
    return {
      url: "https://vue3-course-api.hexschool.io/v2", // 請加入站點
      path: "hexschooljerry", // 請加入個人 API Path
    };
  },
  props: ["productId"],
  methods: {
    deleteProduct() {
      this.axios
        .delete(`${this.url}/api/${this.path}/admin/product/${this.productId}`)
        .then((res) => {
          //   this.deleteAlert(res.data.message, "success");
          alert(res.data.message);
          this.$emit("get-products");
        })
        .catch((er) => {
          alert(er.response.data.message);
        });
    },
    // deleteAlert(message, type) {
    //   var alertPlaceholder = document.getElementById("liveAlertPlaceholder");
    //   var wrapper = document.createElement("div");
    //   wrapper.innerHTML =
    //     '<div class="alert alert-' +
    //     type +
    //     ' alert-dismissible" role="alert">' +
    //     message +
    //     '<button type="button" class="btn-close" data-bs-dismiss="alert" aria-label="Close"></button></div>';
    //   alertPlaceholder.append(wrapper);
    // },
  },
};
</script>

