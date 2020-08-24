<template>
  <div class="add-smoothie container">
    <h2 class="center-align indigo-text" style="font-size: 20px">Add Smoothies</h2>
    <form @submit.prevent="addSmoothie">
      <div class="field title">
        <label>Title</label>
        <input type="text" name="title" v-model="title">
      </div>
      <div v-for="(material, index) in materials" class="text-green" :key="index">
        <label>Ingredient</label>
        <input type="text" name="material" v-model="materials[index]" value="{  material }">
      </div>
      <div class="field add">
        <label>Add Ingredient</label>
        <input type="text" name="add" @keydown.tab.prevent="adding" v-model="another">
      </div>
      <div class="field center-align">
        <p v-if="feedback" class="red">{{ feedback }}</p>
          <button class="btn orange">Add</button>
      </div>
    </form>
  </div>
</template>

<script>
  import db from '@firebase/init'
  export  default {
      name: 'AddSmoothies',
      data(){
        return{
            title: null,
            another: null,
            feedback:null,
            materials:[],

        }
      },
      methods:{
          addSmoothie:function () {
            if(this.title){

            }
            else{
                this.feedback = 'Please  '
            }
          },
          adding:function () {
              if(this.another){
                  this.materials.push(this.another)
                  this.another = null;
                  this.feedback = null;
              }
              else{
                  this.feedback = "Please add smoothies";
              }

          }
      }
  }
</script>


<style>
  .add-smoothie{
    max-width: 40%;
  }


</style>
