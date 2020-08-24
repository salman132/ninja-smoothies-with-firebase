<template>
  <div class="edit-smoothie container">
    <h2 v-if="smoothies">Edit Smoothie {{ smoothies.title }}</h2>
  </div>
</template>

<script>
  import db from '@/firebase/init'
  export default {
      name: 'EditSmoothie',
      data(){
          return{
            smoothies:null,
          }
      },
      created() {
          let ref = db.collection('smoothies').where('slug','==', this.$route.params.smoothie_slug)
          ref.get().then(snapshot =>{
              snapshot.forEach(doc=>{
                  let smoothie = doc.data();
                  smoothie.id = doc.id;
                  this.smoothies = smoothie;
              })
          });



      }
  }
</script>
