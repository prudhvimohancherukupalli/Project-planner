<template>
    <form @submit.prevent="handleSubmit">
        <label>Title</label>
        <input type="text" v-model="title" required >
        <label>Details</label>
        <textarea required v-model="details"></textarea>
        <button>Update Project</button>
      </form>
</template>

<script>
export default {
props:['id'],
data(){
    return {
        title:"",
        details:'',
        url:'http://localhost:3000/projects/'+this.id
    } 
},
mounted(){
    fetch(this.url).then(res=>res.json())
    .then(data=>{
        this.title=data.title
        this.details=data.details
    })
},
methods:{
    handleSubmit(){
    fetch(this.url,{
        method:'PATCH',
        headers:{'Content-Type':'application/json'},
        body:JSON.stringify({title:this.title,details:this.details})
    }).then(()=>{
        this.$router.push('/')
    }).catch(err=>{console.log(err)})
},
}
}
</script>

<style>

</style>