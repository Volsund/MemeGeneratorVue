<template>
  <div class="editor">
    <div  id="imageToDownload"  class="edit-image-container">
      <img :src="imgToEdit" :alt="'Image to edit'" class="edit-image" />

      <div
        style=" color:white;font-weight: bold; height: 400px; width: 400px; position: relative; -webkit-text-stroke: 2px black; -webkit-text-fill-color: white;"
      >
        <vue-draggable-resizable
          :w="300"
          :h="100"
          :x="150"
          @dragging="onDrag"
          @resizing="onResize"
          :parent="true"
          v-bind:style="{ fontSize: topFontSize + 'px' }"
        >
          <p>{{ topText }}<br /></p>
        </vue-draggable-resizable>
        <vue-draggable-resizable
          :w="300"
          :h="100"
          :x="150"
          :y="130"
          @dragging="onDrag"
          @resizing="onResize"
          :parent="true"
          v-bind:style="{ fontSize: bottomFontSize + 'px' }"
        >
          <p>{{ bottomText }}<br /></p>
        </vue-draggable-resizable>
      </div>
    </div>
    <div class="edit-options">
      <form class="edit-form">
        <label for="fname">Text Top</label>
        <input
          type="text"
          id="fname"
          name="fname"
          placeholder="Type text here"
          v-model="topText"
        />
        <label for="range-1">Font Size</label>
        <b-form-input
          id="range-1"
          type="range"
          min="0"
          max="80"
          v-model="topFontSize"
          step="1"
        ></b-form-input>
        <label for="lname">Text Bottom</label>
        <input
          type="text"
          id="lname"
          name="lname"
          v-model="bottomText"
          placeholder="Type text here"
        />
        <label for="range-2">Font Size</label>
        <b-form-input
          id="range-2"
          type="range"
          min="0"
          max="80"
          v-model="bottomFontSize"
          step="1"
        ></b-form-input>
        <label for="fileName">File Name</label>
        <input
          type="text"
          id="fileName"
          name="lname"
          placeholder="Type text here"
          v-model="fileName"
        />
        <div class="button-container">
          <b-button block variant="success" @click="downloadImage"
            >Download</b-button
          >
        </div>
      </form>
    </div>
  </div>
</template>

<script>
// HTML to image import
import { saveAsJpeg } from "save-html-as-image";

export default {
  name: "Gallery",
  props: {
    imgToEdit: {
      type: String,
    },
  },
  data() {
    return {
      fileName: "filenamehere",
      topText: "TOP TEXT",
      topFontSize: 25,
      bottomFontSize: 25,
      bottomText: "BOTTOM",
      width: 0,
      height: 0,
      x: 0,
      y: 0,
    };
  },
  methods: {
    fontInfo: function() {
      return this.fontSizeTop + "px Arial";
    },
    onResize: function(x, y, width, height) {
      this.x = x;
      this.y = y;
      this.width = width;
      this.height = height;
    },
    onDrag: function(x, y) {
      this.x = x;
      this.y = y;
    },
    downloadImage: function() {
      const node = document.getElementById("imageToDownload");
      saveAsJpeg(node, { filename: this.fileName, printDate: false });
    },
  },
};
</script>

<style scoped>
.editor {
  display: flex;
}
.edit-image-container {
  padding-right: 20px;
}
.edit-image {
  width: 400px;
  position: absolute;
}
input[type="text"] {
  width: 100%;
  padding-left: 10px;
  box-sizing: border-box;
}

.button-container {
  padding-top: 10px;
}

.vdr {
  border: none;
}


.edit-image-container {
  font-family: Impact, Haettenschweiler, "Arial Narrow Bold", sans-serif;
}
</style>
