<template>
  <form @submit.prevent="handleEdit">
      <label>Title</label>
      <input type="text" v-model="title" required>
      <label>Details</label>
      <textarea v-model="details" required></textarea>
      <button class="edit">Update Project</button>
  </form>
</template>

<script>
export default {
  props: ['id'],
  name: 'EditProject',
  data() {
      return {
          title: '',
          details: '',
          uri: 'http://localhost:3000/projects/' + this.id
      }
  },
  mounted() {
    fetch(this.uri)
    .then(res => res.json())
    .then(data => {
      this.title = data.title
      this.details = data.details
    })
  },
  methods: {
    handleEdit() {
      fetch(this.uri, {
        method: 'PATCH',
        headers: { 'Content-Type': 'application/json' },
				body: JSON.stringify({ 
          title: this.title,
          details: this.details 
        })        
      }).then(() => {
        this.$router.push('/')
      }).catch(err => console.log(err)) 
    }
  }
}
</script>

<style>
  form button.edit {
    background: rgb(4, 78, 214);
    display: block;
    margin: 20px auto 0;
    color: white;
    padding: 10px;
    border: 0;
    border-radius: 6px;
    font-size: 16px;
    cursor: pointer;
  }
</style>