<template>
  <div class="Api">
   <div class="card"> 

   {{ save.title }}
   {{ save.body }}
    <div class="card-content">
      <input v-model="save.title" type="text" class="input-field" placeholder="Title">
      <input v-model="save.body" type="text" class="input-field" placeholder="Body" >
      <button class="post-button" @click="save_post">Post</button>
    </div>
   </div>

   <div class="list-view" v-for="(post, index) in posts" :key="index">
      <div v-if="!isEditing" class="list-view-item">
        <div class="list-view-subitem">
          {{  post.title }} <br>
        </div>
        <div class="list-view-subitem">
          {{  post.body }} 
        </div>
        <div class="list-view-button-group">
            <button class="list-view-remove-item" @click="delete_post(post.id)">Remove</button>
            <button class="list-view-edit-item" @click="toggle(post)">Edit</button>
        </div>
      </div>
      <div v-else class="list-view-item">
        <div class="list-view-subitem">
          <input type="text" class="input-field" placeholder="title">
          <input type="text" class="input-field" placeholder="body">
        </div>
        <div class="list-view-button-group">
        {{ post.isEditing }}
            <button class="list-view-edit-item" @click="toggle(post)">Edit</button>
        </div>
      </div>
   </div>

  </div>
</template>

<script>
/* eslint-disable */ 
export default {
  name: 'Api',
  props: {
    msg: String
  },
  data(){
    return {
       posts : {},
       save: {},
       url: 'http://192.168.18.2:3000/api/posts/'
    }
  },
  methods: {
    save_post(){
      if(!this.save.title == '' && !this.save.body == ''){
       fetch(this.url, {
        method: 'post',
        headers : {
          'Content-Type': 'application/json'
        },
        body: JSON.stringify({
          title: this.save.title,
          body: this.save.body
        })
      }).then(res => {
        if(res.status == 201){
          this.save.title = "";
          this.save.body = "";
          alert('Data have been successfully been created!');
          this.all_post();
        }
      }).catch(err => alert(err));
      }
      else {
        alert("Please kindly don't leave them blank!")
      }
    },
    delete_post(id){
      fetch(this.url.concat(id), {
        method: 'delete',
        headers : {
          'Content-Type': 'application/json'
        },
      }).then(res => {
        if(res.status == 204){
          alert('Data have been successfully been deleted!');
          this.all_post();
        }
      }).catch(err => console.log(err));
    },
    update_post(){
      // edit business logic here
    },
    get_post(){
      // gets particular post
    },
    async all_post(){
        let response = await fetch(this.url);
        let data = await response.json();
        if(response.ok) {
         this.posts =  Object.keys(data).map(key => {
            return {
              id: key, 
              title: data[key].title,
              body: data[key].body,
              isEditing: false
            }
          })
        }
        else {
          console.log(response.error);
        }
    },
    toggle(post){
      alert("This works ofcourse "+post.isEditing )
    }
  },
  created(){
    this.all_post();
  }
}
</script>
<style scoped>
  @import url('../assets/form.css');  
  @import url('../assets/list.css');
</style>
