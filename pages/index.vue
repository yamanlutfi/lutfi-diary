<template>
  <div class="container">
    <b-col md="12">
      <b-row>
        <b-col md="8">
          <b-card
            v-b-modal.modal-post
            class="br-primary s-primary b-none p-primary bg-white mb-4 t-secondary c-pointer"
          >
            <b-modal
              id="modal-post"
              ref="modal-post"
              title="Write your mood"
              hide-footer
              class="br-primary"
            >
              <div>
                <b-form @submit="onSubmit" v-if="show">
                  <b-form-group
                    id="input-group-1"
                    label="Title:"
                    label-for="input-1"
                  >
                    <b-form-input
                      id="input-1"
                      v-model="form.title"
                      type="text"
                      required
                    ></b-form-input>
                  </b-form-group>

                  <b-form-group
                    id="input-group-2"
                    label="Description:"
                    label-for="input-2"
                  >
                    <b-form-textarea
                      id="textarea"
                      v-model="form.description"
                      rows="3"
                      max-rows="6"
                    ></b-form-textarea>
                  </b-form-group>

                  <b-button type="submit" class="mt-3" variant="danger" block>{{this.button}}</b-button>
                </b-form>
              </div>
            </b-modal>
            <h3>Write your mood here...</h3>
          </b-card>
          <div v-if="$fetchState.pending">
            <LoadingProduct />
          </div>
          <div v-else-if="$fetchState.error">Terjadi kesalahan :(</div>
          <div v-else>
            <div v-for="post of posts" :key="posts.title">
              <b-card
                :title="post.title"
                img-alt="Image"
                img-top
                tag="article"
                class="br-primary s-primary b-none p-primary bg-white mb-4"
              >
                <b-card-text class="text-product">
                  {{ post.description }}
                </b-card-text>
                <label class="t-secondary">{{ post.date }}</label>
              </b-card>
            </div>
          </div>
        </b-col>
        <b-col md="4">
          <div class="s-primary ads bg-white br-primary">
            <img class="img-fluid br-primary" src="https://image.freepik.com/free-vector/opening-soon-background-flat-style_23-2148249882.jpg"/>
          </div>
        </b-col>
      </b-row>
    </b-col>
  </div>
</template>

<script>
import LoadingProductVue from "../components/product/LoadingProduct.vue";
export default {
  data() {
    return {
      posts: [],
      form: {
        title: '',
        description: '',
        date: '',
        id: ''
      },
      button: 'Post',
      foods: [{ text: 'Select One', value: null }, 'Carrots', 'Beans', 'Tomatoes', 'Corn'],
      show: true
    };
  },
  layout: "frontend", // kita bisa menggunakan layout lain tergantung pada apa yg ditulis pada line ini
  components: {
    LoadingProduct: LoadingProductVue,
  },
  fetchOnServer: false, // memanggil fetch hanya pada sisi klien
  async fetch() {
    // GET API
    this.posts = await fetch(
      "https://yamandiary.herokuapp.com/posts"
    ).then((res) => res.json());
  },
  methods: {
    post() {
      this.$refs["modal-post"].hide();
    },
    async onSubmit(event) {
      event.preventDefault()
      console.log(this.posts)
      
      // Get Date
      var today = new Date();
      var dd = String(today.getDate()).padStart(2, "0");
      var mm = String(today.getMonth() + 1).padStart(2, "0"); //January is 0!
      var yyyy = today.getFullYear();
      today = mm + "/" + dd + "/" + yyyy;

      // POST API
      this.form.date = today
      this.form.id = this.posts.length*1+1
      this.button = 'Loading'
      this.posts = await fetch(
        "https://yamandiary.herokuapp.com/posts",
        {
          method: 'Post',
          body: JSON.stringify(this.form),
          headers: {
            "Content-Type": "application/json",
          }
        }
      ).then((res) => {
        this.form.title = ''
        this.form.description = ''
        this.$fetch()
        this.$refs["modal-post"].hide()
      });
    }
  },
};
</script>