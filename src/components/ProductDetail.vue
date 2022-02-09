<template>
  <div
    id="detailModal"
    ref="productModal"
    class="modal fade"
    tabindex="-1"
    aria-labelledby="detailModalLabel"
    aria-hidden="true"
  >
    <div class="modal-dialog modal-xl">
      <div class="modal-content border-0">
        <div class="modal-header bg-dark text-white">
          <h5 id="detailModalLabel" class="modal-title">
            <span>單一產品細節</span>
          </h5>
          <button
            type="button"
            class="btn-close"
            data-bs-dismiss="modal"
            aria-label="Close"
          ></button>
        </div>
        <div class="modal-body">
          <div class="row">
            <div class="col-md">
              <div v-if="tempProduct.title">
                <div class="card mb-4">
                  <img
                    class="card-img-top primary-image"
                    :src="tempProduct.imageUrl"
                    alt="Card image cap"
                  />
                  <div class="card-body">
                    <h5 class="card-text">
                      {{ tempProduct.name }}
                      <span class="badge bg-primary ms-2"
                        >{{ tempProduct.category }}
                      </span>
                    </h5>

                    <p class="card-text">
                      商品描述 :
                      {{ tempProduct.description }}
                    </p>
                    <p class="card-text">
                      商品內容 :
                      {{ tempProduct.content }}
                    </p>
                    <p class="card-text">
                      {{ tempProduct.price }}
                      <del>{{ tempProduct.origin_price }}</del>
                      {{ tempProduct.unit }} / 元
                    </p>
                  </div>
                </div>
                <span
                  v-for="(item, index) in tempProduct.imagesUrl"
                  :key="index"
                >
                  <img
                    :src="item"
                    alt=""
                    class="images m-2"
                    style="float: left"
                  />
                </span>
              </div>
              <p v-else>請選擇一個商品查看</p>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
  <!-- UpdateOrAddModal end -->
</template>

<script>
export default {
  data() {
    return {
      url: "https://vue3-course-api.hexschool.io/v2", // 請加入站點
      path: "hexschooljerry", // 請加入個人 API Path
    };
  },
  props: ["tempProduct"],
  methods: {},
};
</script>


<style scoped>
h2,
h5,
table,
p {
  text-align: left;
}

img {
  object-fit: contain;
  max-width: 100%;
}

.primary-image {
  height: 300px;
}

.images {
  height: 150px;
}
</style>


