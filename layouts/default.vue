<template>
  <div>
    <img v-if="!backgroundImage" src="~assets/wallpaper.gif" width="100%"/>
    <img v-else :src="backgroundImage" width="100%"/>
    <Nuxt />
    <b-field v-if="!uploadFile">
      <b-upload v-model="uploadFile" size="is-small" drag-drop @input="check(uploadFile, $event)" expanded>
        <section class="section">
          <div class="content has-text-centered">
            <p>
              <b-icon icon="upload" size="is-large">
              </b-icon>
            </p>
            <p>Upload background</p>
          </div>
        </section>
      </b-upload>
    </b-field>
  </div>
</template>

<script>
export default {
  name: 'DefaultLayout',
  data() {
    return {
      backgroundImage: undefined,
      uploadFile: undefined,
      items: [
        {
          title: 'Home',
          icon: 'home',
          to: { name: 'index' }
        },
        {
          title: 'Inspire',
          icon: 'lightbulb',
          to: { name: 'inspire' }
        }
      ]
    }
  },
  methods:{
    check(item, file){
      var reader = new FileReader();

      reader.onload = (e) => {
        this.backgroundImage = e.target.result;
      };
      reader.readAsDataURL(file);

      console.log(this.backgroundImage);
    },
  }
}
</script>

<style>
</style>
