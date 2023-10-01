
<template>
  <div class="card">
    <a href="#" target="_blank">
      <img :src="animeImageUrl" :alt="anime.title" />
      <h3>{{ anime.title }}</h3>
    </a>
  </div>
</template>

<script>
export default {
  props: ['anime'],
  computed: {
  animeImageUrl() {
  
    const imageFormat = 'jpg'; 
    const image = this.anime.images[imageFormat];
   
    if (image && image.large_image_url) {
      return image.large_image_url;
    } else {
    
      return 'https://example.com/placeholder.jpg'; 
    }
  },
},

};
</script>



