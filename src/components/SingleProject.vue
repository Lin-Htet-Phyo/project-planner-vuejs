<template>
  <div class="project" :class="{ complete: project.complete }">
    <div class="flexing">
      <div @click="showDetail = !showDetail">
        <h3>{{ project.title }}</h3>
      </div>
      <div>
        <span class="material-icons" @click="deleteProject">delete</span>
        <router-link :to="{name:'editProject', params: {id: project.id}}">
          <span class="material-icons">edit</span>
        </router-link>
        <span class="material-icons" @click="completeProject">done</span>
      </div>
    </div>
    <p v-if="showDetail">{{ project.detail }}</p>
  </div>
</template>

<script>
export default {
  props: ["project"],
  data() {
    return {
      showDetail: false,
      api: "http://localhost:3000/projects/",
    };
  },
  methods: {
    deleteProject() {
      fetch(this.api + this.project.id, { method: "DELETE" })
        .then(() => {
          this.$emit("delete", this.project.id);
        })
        .catch((err) => {
          console.log(err);
        });
    },
    completeProject() {
      let updateComplete = this.api + this.project.id;
      fetch(updateComplete, {
        method: "PATCH",
        headers: {
          "Content-Type":"application/json"
        },
        body: JSON.stringify(
          {
            complete: !this.project.complete
          }
        )
      })
        .then(() => {
          this.$emit("complete", this.project.id);
        })
        .catch((err) => {
          console.log(err);
        });
    },
  },
};
</script>

<style>
.project {
  padding: 20px;
  background-color: #f2f2f2;
  margin: 10px;
  border-left: 6px solid crimson;
  border-radius: 5px;
}

h3 {
  color: indigo;
  cursor: pointer;
}

.flexing {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

span {
  margin-left: 10px;
}
span:hover {
  color: #777;
  cursor: pointer;
}

.complete {
  border-left-color: green;
}
</style>