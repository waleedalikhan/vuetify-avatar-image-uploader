<template>
  <v-container>
    <h1 class="display-3 text-center">Upload Profile Pic</h1>
    <v-row justify="center" v-if="objectUrl">
      <v-col class="text-right" col="6">
        <div class="image-container d-block mx-auto mb-10">
          <img class="image-preview text-right" ref="source" :src="objectUrl" />
        </div>
      </v-col>
      <v-col cols="6" class="text-left">
        <v-avatar width="200" height="200" class="d-block mx-auto mb-10">
          <img :src="previewCropped" />
        </v-avatar>
      </v-col>
    </v-row>
    <v-btn @click="selectImg" class="text-center mx-auto">Upload Avatar</v-btn>
    <v-btn @click="removeImg" class="text-center ml-8" v-if="objectUrl">remove Avatar</v-btn>
    <v-file-input
      class="my-4"
      accept="image/png, image/jpeg"
      label="File"
      placeholder="Select a file"
      id="fileSelect"
      :show-size="1024"
      @change="setupCropper"
      style="display: none"
    ></v-file-input>
  </v-container>
</template>

<script>
import Cropper from "cropperjs";

export default {
  data() {
    return {
      cropper: null,
      objectUrl: null,
      previewCropped: null,
      selectedFile: null
    };
  },

  methods: {
    setupCropper(selectedFile) {
      if (this.objectUrl) {
        window.URL.revokeObjectURL(this.objectUrl);
      }

      if (!selectedFile) {
        this.cropper = null;
        this.objectUrl = null;
        this.previewCropped = null;
        return;
      }

      this.objectUrl = window.URL.createObjectURL(selectedFile);
      this.$nextTick(this.setupCropperInstance);
    },
    setupCropperInstance() {
      this.cropper = new Cropper(this.$refs.source, {
        aspectRatio: 1,
        crop: () => {
          const canvas = this.cropper.getCroppedCanvas();
          this.previewCropped = canvas.toDataURL(
            "image/png",
            "image/jpeg",
            "image/jpg"
          );
        }
      });
    },
    selectImg: function() {
      document.getElementById("fileSelect").click();
    },
    removeImg: function() {
      if (this.cropper) {
        this.cropper.destroy();
        this.cropper = null;
        this.objectUrl = null;
        this.previewCropped = null;
        return;
      }
    }
  }
};
</script>

<style scoped>
.image-container {
  width: 200px;
  height: 200px;
}
</style>