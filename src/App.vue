<template>
  <v-app>
    <v-content>
      <ImageCropper />
    </v-content>
  </v-app>
</template>

<script>
import ImageCropper from "./components/ImageCropper";

export default {
  name: "App",
  components: {
    ImageCropper
  },

  data: () => ({
    //
  })
};
</script>
