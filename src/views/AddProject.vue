<template>
  <form @submit.prevent="handleSubmit">
    <label>Title</label>
    <input type="text" v-model="title" required >
    <label>Details</label>
    <textarea required v-model="details"></textarea>
    <button>Add Project</button>
  </form>
</template>

<script>
export default {
data(){
    return {
        title:'',
        details:'',
    }
},
methods:{
    async handleSubmit(){
        const response = await fetch('http://localhost:3000/projects');
        const projects = await response.json();
        const id= projects.length + 1;
        let project={
            title:this.title,
            details:this.details,
            complete:false,
            id:id
        }
        fetch('http://localhost:3000/projects',{
            method:'POST',
            headers:{'Content-Type':'application/json'},
            body:JSON.stringify(project)
        })
        .then(()=>{
            this.$router.push('/')
        })
        .catch((err)=>{console.log(err.message)})
    }

}
}
</script>

<style>
form{
    background: white;
    padding: 20px;
    border-radius: 10px;
    align-content: center;
}
label{
    display: block;
    color: #bbb;
    text-transform: uppercase;
    font-size: 14px;
    font-weight: bold;
    letter-spacing: 1px;
    margin: 20px 0 10px 0;
}
input{
    padding: 10px;
    border:0;
    border-bottom: 1px solid #ddd;
    width: 100%;
    box-sizing: border-box;
}
textarea{
    border: 1px solid #ddd;
    width: 100%;
    box-sizing: border-box;    
}
form button{
    display: block;
    margin: 20px auto 0;
    columns: white;
    font-size: 16px;
    padding: 10px;
    background: #00ce89;
    width: 30%;
    justify-content: space-around;
}
</style>