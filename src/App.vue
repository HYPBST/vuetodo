<template>
  <div id="app">
    <table>
      <thead>
        <tr>
          <th>Id</th>
          <th>Title</th>
          <th>On display</th>
          <th>Year</th>
          <th>Operations</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="painting in paintings" v-bind:key="painting.id">
          <td>{{painting.id}}</td>
          <td>{{painting.title}}</td>
          <td>{{painting.on_display}}</td>
          <td>{{painting.year}}</td>
          <td><button @click="deletePainting(painting.id)">Delete</button></td>
        </tr>
        <tr>
          <td><input type="hidden" v-model="painting.id"></td>
          <td><input type="text" v-model="painting.title"></td>
          <td><input type="checkbox" v-model="painting.on_display"></td>
          <td><input type="number" v-model="painting.year"></td>
          <td><button @click="letrehoz">Létrehoz</button></td>
        </tr>
      </tbody>
    </table>
    <br>
    <button @click="loadData">Adatok betöltése</button>
  </div>
</template>

<script>


export default {
  name: 'App',
  components: {
  },
  data() {
    return {
      painting:{
        id:null,
        title:'',
        on_display:false,
        year:''
      },
      paintings:[]
    }
  },
  methods: {
   async loadData(){
    let Response = await fetch('http://127.0.0.1:8000/api/paintings')
    let data=await Response.json()
    this.paintings = data;
   },
    async deletePainting(id){
     let Response=await fetch(`http://127.0.0.1:8000/api/paintings/${id}`,{
       method: 'DELETE'
     })
     console.log(Response)
     await this.loadData()
   },
   async letrehoz(){
     await fetch('http://127.0.0.1:8000/api/paintings',{
       method:'POST',
       headers: {
         'Content-Type': 'application/json',
         'Accept': 'application/json'
       },
       body: JSON.stringify(this.painting)
     })
     await this.loadData()
   }
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>