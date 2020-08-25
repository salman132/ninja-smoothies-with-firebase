<template>
  <div class="edit-smoothie container">
    <h2 v-if="smoothies">Edit Smoothie {{ smoothies.title }}</h2>

    <form @submit.prevent="editSmoothie">
      <div class="field title">
        <label>Title</label>
        <input type="text" name="title" v-if="smoothies" v-model="smoothies.title">
      </div>
      <div class="check" v-if="smoothies">
        <div v-for="(material, index) in smoothies.materials" class="text-green" :key="index">
          <label>Ingredient</label>
          <input type="text" name="material"  v-model="smoothies.materials[index]">
          <i class="material-icons delete" @click="deleteMaterial(material)">delete</i>
        </div>

      </div>

      <div class="field add">
        <label>Add Ingredient</label>
        <input type="text" name="add" @keydown.tab.prevent="adding" v-model="another">
      </div>
      <div class="field center-align">
        <p v-if="feedback" class="red">{{ feedback }}</p>
        <button class="btn orange">Update</button>
      </div>
    </form>
  </div>
</template>

<script>
  import db from '@/firebase/init'
  export default {
      name: 'EditSmoothie',
      data(){
          return{
            smoothies:null,
            another:null,
            feedback: null,


          }
      },
      created() {
          let ref = db.collection('smoothies').where('slug','==', this.$route.params.smoothie_slug)
          ref.get().then(snapshot =>{
              snapshot.forEach(doc=>{
                  this.smoothies = doc.data();
                  this.smoothies.id = doc.id;

              })
          });



      },
      methods:{
          editSmoothie:function () {
              if(this.smoothies.title){
                  this.feedback = null;
                  db.collection('smoothies').doc(this.smoothies.id).update({
                      title: this.smoothies.title,
                      materials: this.smoothies.materials,
                      slug: this.makeSlug(this.smoothies.title)
                  }).then(()=>{
                      this.$router.push({name : 'Index'});

                  }).catch(error =>{
                      console.log(error);
                  })
              }
              else{
                  this.feedback = 'Please enter the title'
              }
          },
          adding:function () {
              if(this.another){
                  this.smoothies.materials.push(this.another)
                  this.another = null;
                  this.feedback = null;
              }
              else{
                  this.feedback = "Please add smoothies";
              }


          },

          deleteMaterial:function(name){
              this.smoothies.materials  =this.smoothies.materials.filter(material =>{
                  return material != name
              })
          },
          makeSlug:function (text) {

              return text.toLowerCase().replace(/ /g,'-').replace(/[^\w-]+/g,'');
          }


      }
  }
</script>


<style>
  .edit-smoothie{
    max-width: 40%;
  }

  .delete{
    cursor: pointer;
    color: darkred;
  }
</style>
