<template>
  <form @submit.prevent="handleSubmit">
    <label for="title">Title:</label>
    <input type="text" id="title" v-model="project.title" required />

    <label for="details">Details:</label>
    <textarea id="details" v-model="project.details" required></textarea>

    <button>Update project</button>
  </form>
</template>
  
  <script>
export default {
  props: ["id"],
  data() {
    return {
      project: {
        title: "",
        details: "",
      },
    };
  },
  methods: {
    handleSubmit() {
      let project = {
        title: this.project.title,
        details: this.project.details,
      };

      fetch(`http://localhost:3000/projects/${this.id}`, {
        method: "PATCH",
        headers: { "Content-Type": "application/json" },
        body: JSON.stringify(project),
      })
        .then(() => this.$router.push("/"))
        .catch((err) => console.log(err));
    },
  },
  mounted() {
    fetch(`http://localhost:3000/projects/${this.id}`)
      .then((response) => response.json())
      .then((data) => (this.project = data))
      .catch((error) => console.error(error));
  
  },
};
</script>
  
  <style>
form {
  background: white;
  padding: 20px;
  border-radius: 10px;
}
label {
  display: block;
  color: #bbb;
  text-transform: uppercase;
  font-size: 14px;
  letter-spacing: 1px;
  margin: 20px 0 10px 0;
}
input {
  padding: 10px;
  border: 0;
  border-bottom: 1px solid #ddd;
  width: 100%;
  box-sizing: border-box;
}
textarea {
  border: 1px solid #ddd;
  padding: 10px;
  width: 100%;
  box-sizing: border-box;
}
form button {
  display: block;
  margin: 20px auto 0;
  background: #00ce89;
  color: white;
  padding: 10px;
  border: 0;
  border-radius: 6px;
  font-size: 16px;
}
</style>