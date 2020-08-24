<template>
  <div class="index container">
    <div class="card" v-for="smooth in smoothies" :key="smooth.id">
        <div class="card-content">
          <div class="material-icons delete" @click="deleteSmoothies(smooth.id)">delete</div>
          <h2 class="indigo-text">{{ smooth.title}}</h2>
          <ul class="material">
            <li v-for="(material , index) in smooth.materials" :key="index">
              <span class="chip">
                {{  material }}
              </span>
            </li>
          </ul>
        </div>
    </div>


  </div>
</template>

<script>
  import db from '@/firebase/init'
export default {
  name: 'Index',
  data () {
    return {
      smoothies:[],
    }
  },

    methods:{
      deleteSmoothies:function (id) {
          console.log(id);
          db.collection('smoothies').doc(id).delete()
              .then(()=>{
                  this.smoothies = this.smoothies.filter(smoothie =>{
                      return smoothie.id !== id;
                  })

              }).catch(error =>{
                  console.log(error)
          })

      }
    },
    created() {
      //fetch data from firestore
        db.collection('smoothies').get().then(querySnapshot =>{
            querySnapshot.forEach(doc =>{

              let smoothie = doc.data();
              smoothie.id = doc.id;
              this.smoothies.push(smoothie);

            })

        })


    }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style>
.index{
  display: grid;
  grid-template-columns: 1fr 1fr 1fr;
  grid-gap: 30px;
  margin-top: 60px;
}
  .index h2{
    font-size: 1.8em;
    text-align: center;
    margin-top: 0;
  }

  .index .material{
    margin: 30px auto;
  }

  .index .material li{
    display: inline-block;
  }
  .index .delete{
    position: absolute;
    top: 4px;
    right: 4px;
    cursor: pointer;
    color: #ffaaaa;
    font-size: 1.4em;
  }
</style>
