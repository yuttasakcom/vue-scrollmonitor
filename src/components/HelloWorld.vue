<template>
  <div>
    <h2>Hello</h2>

    <div>
      <ul>
        <li v-for="item in items" :key="item.id">
          {{item}}
        </li>
      </ul>
    </div>

    <div v-if="loading">Loading...</div>
    <div id="product-list-bottom"></div>
  </div>
</template>

<script>
import axios from "axios";
import scrollMonitor from "scrollmonitor/scrollMonitor";

export default {
  data: () => ({
    page: 1,
    items: [],
    loading: false
  }),
  created() {
    console.log("created", this.page);
    axios
      .get(`https://jsonplaceholder.typicode.com/albums/${this.page}/photos`)
      .then(res => res.data)
      .then(data => {
        this.items = data;
      })
      .catch(err => console.log(err));
  },
  mounted() {
    let self = this;
    const myElement = document.getElementById("product-list-bottom");
    const elementWatcher = scrollMonitor.create(myElement);

    elementWatcher.enterViewport(function() {
      self.page++;
      self.appendItems();
    });
  },
  methods: {
    appendItems() {
      this.loading = true;
      axios
        .get(`https://jsonplaceholder.typicode.com/albums/${this.page}/photos`)
        .then(res => res.data)
        .then(data => {
          this.items.push(...data);
          this.loading = false;
        })
        .catch(err => console.log(err));
    }
  }
};
</script>
