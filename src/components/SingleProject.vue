<template>
  <div class="project" :class="{complete :project.complete}">
      <div class="actions" >
          <h3 @click="toggleShowDetails">{{ project.title }}</h3>
         <div class="icons">
             <router-link :to="{name : 'EditProject',params:{id:project.id}}">
                 <span class="icon"><i class="fas fa-pencil-alt"></i></span>
             </router-link>
          
          <span class="icon" @click="deleteProject"><i class="fas fa-trash-alt"></i></span>
           <span class="icon tick" @click="toggleComplete" ><i class="fas fa-check"></i></span>
           </div>
      </div>
      <div v-if="showDetails">
          <p> {{ project.details}} </p>
      </div>
 
  </div>
</template>

<script>
export default {
   props:['project'],
   data() {
       return{
           showDetails:false,
           uri :'http://localhost:3000/projects/' + this.project.id
       }
   },
   methods:{
       toggleShowDetails() {
           this.showDetails =!this.showDetails

       },
       deleteProject() {
           fetch(this.uri,{method: 'DELETE'})
           .then(() => this.$emit('delete',this.project.id))
           .catch(err =>console.log(err))

       },
       toggleComplete() {
           fetch(this.uri, {
               method: 'PATCH',
               headers: {'Content-Type': 'application/json'},
               body:JSON.stringify({complete:!this.project.complete})
               }).then(() => {
                   this.$emit('complete',this.project.id)
               }).catch((err) => console.log(err))  
       }
   }
}

</script>

<style>
.project{
    margin: 20px auto;
    background: white;
    padding: 10px 20px;
    border-radius: 4px;
    box-shadow: 1px 2px 3px rgba(0,0,0,0.05);
    border-left: 8px solid #e90074;
}

h3{
    cursor: pointer;
}

.actions{
    display: flex;
    align-items: center;
    justify-content: space-between;
}
.icon{
    font-size: 24px;
    margin-left: 10px;
    color: #bbb;
    cursor: pointer;
}

.icon:hover{
    color:#777;
}

.project.complete{
    border-left:8px solid #00ce89;
}

.project.complete  .tick{
    color: #00ce89;
}

</style> 