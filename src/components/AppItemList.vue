<template>
  <div>
    <h5>{{ title }} <span class="badge badge-info">{{ items.length }}</span></h5>
    <div class="card">
      <div class="card-body">
        <ul class="list-group">
          <li class="list-group-item" v-for="item in items" v-bind:key="item">
            <div class="row">
              <div class="col-md">
                {{ item }}
              </div>
              <div class="col-md text-right">
                <button class="btn btn-info" v-on:click="deleteItem(item)">
                  <span class="fa fa-trash"></span>
                </button>
              </div>
            </div>
          </li>
        </ul>
        <br/>
        <div class="input-group">
          <input
            class="form-control"
            ref="addItemButton"
            type="text"
            v-model="item"
            v-on:keydown.enter="addItem(item)"
            placeholder="Digite o item">
          <div class="input-group-append">
            <button class="btn btn-info" v-on:click="addItem(item)"><span class="fa fa-plus"></span></button>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "AppItemList",
  props: {
    title: String,
    items: Array
  },
  data() {
    return {
      item: "",
    };
  },
  methods: {
    addItem(item) {
      // TODO: learn proper validation
      if (this.item != "" && !this.items.includes(item)) {
        this.items.push(item);
        this.$emit("addItem", item);
      }
      this.item = "";
      this.$refs.addItemButton.$el.focus();
    },
    deleteItem(item){
      this.items.splice(this.items.indexOf(item), 1);
      this.$emit("deleteItem", item);
    }
  }
};
</script>

<style scoped>
</style>
