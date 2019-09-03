<template>
  <div class="Api">
   <div class="card"> 
    <div class="card-content">
      <input v-model="save.title" type="text" class="input-field" placeholder="Title">
      <input v-model="save.body" type="text" class="input-field" placeholder="Body" >
      <button class="post-button" @click="save_post">Post</button>
    </div>
   </div>

   <div class="list-view" v-for="(post, index) in posts" :key="index">
      <div v-if="!post.isEditing" class="list-view-item">
        <div class="list-view-subitem">
          {{  post.title }} 
        </div>
        <div class="list-view-subitem">
          {{  post.body }} 
        </div>
        <div class="list-view-button-group">
            <button class="list-view-remove-item" @click="delete_post(post.id)">Remove</button>
            <button class="list-view-edit-item" @click="toggleEdit(post)">Edit</button>
        </div>
      </div>
      <div v-else class="list-view-item">
        <div class="list-view-subitem"> 
          <input :value="post.title" @input="post.title = $event.target.value" type="text" class="input-field" placeholder="title">
          <input :value="post.body"  @input="post.body = $event.target.value" type="text" class="input-field" placeholder="body">
        </div>
        <div class="list-view-button-group">
            <button class="list-view-edit-item" @click="update_post(post)">Update</button>
            <button class="list-view-done-item" @click="toggleEdit(post)">Done</button>
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
     toggleEdit(post){
      post.isEditing = !post.isEditing;
    },
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
    update_post(post){
      fetch(this.url.concat(post.id), {
        method: 'PUT',
        body: JSON.stringify({
          title: post.title,
          body: post.body,
        }),
        headers: {
          "Content-type": "application/json"
        }
       })
        .then(response => {
          if(response.ok){
            alert("Data is updated!");
            post.isEditing = !post.isEditing;
          }
        })

    },
    async all_post(){
        let response = await fetch(this.url);
        let data = await response.json();
        if(response.ok) {
         this.posts =  Object.keys(data).map(key => {
            return {
              id: data[key].id, 
              title: data[key].title,
              body: data[key].body,
              created_at: data[key].created_at,
              updated_at: data[key].updated_at,
              isEditing: false
            }
          })
        }
        else {
          console.log(response.error);
        }
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
