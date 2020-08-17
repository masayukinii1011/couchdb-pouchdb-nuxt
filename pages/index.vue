<template>
  <div class="container">
    <input type="text" v-model="newTitle" />
    <input type="text" v-model="newId" />
    <button @click="add()">add</button>
    <button @click="test()">test</button>
    <div v-for="(row, index) in rows" :key="index">{{row.id}} {{row.doc.title}}</div>
  </div>
</template>

<script>
import PouchDB from "pouchdb";
export default {
  data: function () {
    return {
      movies: "",
      rows: "",
      newTitle: "",
      newId: "",
    };
  },
  methods: {
    add: function () {
      this.movies
        .put({
          _id: this.newId,
          title: this.newTitle,
        })
        .then(function (response) {
          console.log("Success", response);
        })
        .then(function (err) {
          console.log("Error", err);
        });
    },
    test: function () {
      console.log(this.rows);
    },
  },
  mounted: function () {
    let self = this;
    this.movies = new PouchDB("Movies");
    this.movies.allDocs({ include_docs: true }).then(function (docs) {
      let data = [];
      docs.rows.forEach(function (doc) {
        data.push(doc);
      });
      self.rows = data;
    });
  },
};
</script>