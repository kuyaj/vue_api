<template>
  <div class="Api">
    <ul v-for="post in posts">
      <li>{{ post.id }}</li>
      <li>{{ post.title }}</li>
      <li>{{ post.body }}</li>
    </ul>
{{ textInput.id }}
    <input type="text" v-model="textInput.id" placeholder="Put ID to delete">
    <button @click="delete_post">Delete</button>
  </div>
</template>

<script>
export default {
  name: 'Api',
  props: {
    msg: String
  },
  data(){
    return {
       posts : {},
       textInput: {}
    }
  },
  methods: {
    save_post(){
      fetch('http://localhost:3000/api/posts', {
        method: 'post',
        headers : {
          'Content-Type': 'application/json'
        },
        body: JSON.stringify({
          title: "Test",
          body: "Test"
        })
      })
    },
    delete_post(){
      fetch('http://localhost:3000/api/posts/'.concat(this.textInput.id), {
        method: 'delete',
        headers : {
          'Content-Type': 'application/json'
        },
      })
    },
    edit_post(){
      // edit business logic here
    }
  },
  async created(){
    let response = await fetch('http://localhost:3000/api/posts');
    let data = await response.json();
    if(response.ok) {
      this.posts = data;
    }
    else {
      console.log(response.error);
    }
  }
}
</script>
<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
 ul {
   list-style-type: none;
 }
</style>
