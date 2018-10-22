<template>
  <div class="home">
  <h1 class="text-white bg-success mt-3 p-4 rounded">Full-Stack message board</h1>
  <button v-show="!displayForm" @click="displayForm = !displayForm"  type="button" class="btn btn-primary">New Message ?</button>
  <div v-if="errors" class="alert alert-dismissible alert-danger">
    <button type="button" class="close" data-dismiss="alert">&times;</button>
    <strong>Oh snap!</strong> {{errors}} 
  </div>
  <form v-if="displayForm" class="my-3 border-bottom" @submit.prevent="addMessage">
    <div class="form-group">
      <label for="username">Username:</label>
      <input v-model="newMessage.username" type="text" class="form-control" id="username" placeholder="Your username" required>
    </div> 
    <div class="form-group">
      <label for="subject">Subject:</label>
      <input v-model="newMessage.subject" type="text" class="form-control" id="subject" placeholder="Enter Subject" required>
    </div> 
     <div class="form-group">
      <label for="message">Message:</label>
      <textarea v-model="newMessage.message" class="form-control" id="message" placeholder="Enter Your Message" ></textarea>
    </div>
    <div class="form-group">
      <label for="subject">Image URL:</label>
      <input v-model="newMessage.imgURL" type="url" class="form-control" id="subject" placeholder="Enter Image URL">
    </div>
    <button type="submit" class="btn btn-primary">Primary</button>
  </form>

    <ul class="list-unstyled my-4">
      <li class="media mb-2 " 
          v-for="message in messages"
          :key="message._id"
      >
        <img v-if="message.imgURL" class="mr-3" :src="message.imgURL" :alt="message.subject">
        <div class="media-body">
          <h5 class="mt-0 mb-1">{{message.subject}}</h5>
          <h6>By: {{message.username}}</h6>
          <p>{{message.message}}</p>
          <br>
          Created at: {{message.created}}
        </div>
         <hr>
      </li>
     
    </ul>
  </div>
</template>

<script>
const API_URL = 'http://localhost:5555/messages';

export default {
  name: 'home',
  data() {
    return {
      messages: [],
      newMessage: {
        username: 'Anonymous',
        subject: '',
        message: '',
        imgURL: ''
      },
      errors: '',
      displayForm: false
    }
  },
  methods: {
    addMessage() {
      fetch(API_URL, {
        method: 'POST',
        body: JSON.stringify(this.newMessage),
        headers: {
          'content-type': 'application/json'
        }
      }).then(response => response.json())
        .then(result => {
          if(result.details) {
            const errors = result.details.map(detail => detail.message).join(' ');
            this.errors = errors;
          } else {
            this.messages.push(this.newMessage);

          }
          

         });
    }
  },
  mounted() {
    fetch(API_URL)
    .then(res => res.json())
    .then(data => {
      this.messages = data;
    });
  },
}
</script>

<style lang="scss" scoped>
   img {
    max-width: 300px;
    height: auto;
  }

</style>

