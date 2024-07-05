<template>
  <div class="project" :class="{ complete: project.complete }">
    <div class="actions" @click="toggleDetails">
      <h3>{{ project.title }}</h3>
    </div>

    <div class="icons">
      <router-link :to="{ name: 'editProject', params: { id: project.id }}">
        <span class="material-icons"> edit </span>
      </router-link>
      <span class="material-icons" @click="deleteProject"> delete </span>
      <span class="material-icons tick" @click="toggleComplete"> done </span>
    </div>

    <div v-show="showDetails" class="details">
      <p>{{ project.details }}</p>
    </div>
  </div>
</template>

<script>
export default {
  props: ["project"],
  data() {
    return {
      showDetails: false,
      uri: "http://localhost:3000/projects/" + this.project.id,
    };
  },
  methods: {
    toggleDetails() {
      this.showDetails = !this.showDetails;
    },
    deleteProject() {
      fetch(this.uri, {
        method: "DELETE",
      })
        .then((response) => response.json())
        .then(() => this.$emit("delete", this.project.id))
        .catch((error) => console.error(error));
    },
    toggleComplete() {
      fetch(this.uri, {
        method: "PATCH",
        headers: {
          "Content-Type": "application/json",
        },
        body: JSON.stringify({ complete: !this.project.complete }), // send the reverse of the current value (send true if false, etc...)
      })
        .then((response) => response.json())
        .then(() => this.$emit("complete", this.project.id))
        .catch((error) => console.error(error));
    },
  },
};
</script>

<style>
.project {
  margin: 20px auto;
  background: white;
  padding: 10px 20px;
  border-radius: 4px;
  box-shadow: 1px 2px 3px rgba(0, 0, 0, 0.05);
  border-left: 4px solid #e90074;
}
h3 {
  cursor: pointer;
}

.action {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.material-icons {
  font-size: 24px;
  margin-left: 10px;
  color: #bbb;
  cursor: pointer;
}

.material-icons:hover {
  color: #777;
}

.project.complete {
  border-left-color: #00ce89;
}

.project.complete .tick {
  color: #00ce89;
}
</style>