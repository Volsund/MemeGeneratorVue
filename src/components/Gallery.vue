<template>
  <div class="container">
    <b-row class="mt-5" v-if="!images.length">
      <b-col>
        <div>
          <b-spinner label="Loading images" variant="primary"></b-spinner>
        </div>
      </b-col>
    </b-row>

    <div class="gallery">
      <template v-for="image in images">
        <img
          :src="image.url"
          :key="image.id"
          :alt="'Image of ${image.name} meme'"
          class="meme-thumb"
          v-b-modal.modal-1
          @click="imgToEdit = image.url"
        />
      </template>
    </div>
    <b-modal id="modal-1" title="Personalize Your Meme!" size="lg">
      <Editor :imgToEdit="imgToEdit" />
    </b-modal>
  </div>
</template>

<script>
import Editor from "@/components/Editor.vue";

export default {
  name: "Gallery",
  components: {
    Editor,
  },
  data() {
    return {
      images: [],
      imgToEdit: "",
    };
  },
  created() {
    fetch("https://api.imgflip.com/get_memes")
      .then((response) => response.json())
      .then((response) => (this.images = response.data.memes))
      .catch((err) => console.log(err));
  },
};
</script>

<style scoped>
.gallery {
  line-height: 0;
  -webkit-column-count: 5;
  -webkit-column-gap: 0px;
  -moz-column-count: 5;
  -moz-column-gap: 0px;
  column-count: 5;
  column-gap: 0px;
}

.gallery img {
  width: 100% !important;
  height: auto !important;
  cursor:pointer;
}
@media (max-width: 1200px) {
  .gallery {
    -moz-column-count: 4;
    -webkit-column-count: 4;
    column-count: 4;
  }
}
@media (max-width: 1000px) {
  .gallery {
    -moz-column-count: 3;
    -webkit-column-count: 3;
    column-count: 3;
  }
}
@media (max-width: 800px) {
  .gallery {
    -moz-column-count: 2;
    -webkit-column-count: 2;
    column-count: 2;
  }
}
@media (max-width: 400px) {
  .gallery {
    -moz-column-count: 1;
    -webkit-column-count: 1;
    column-count: 1;
  }
}

</style>
