<template>
  <div class="ui grid sixteen wide" style="margin: 1rem;">
      <div class="ui column one wide green">#</div>
      <div class="ui column six wide green">Description</div>
      <div class="ui column six wide green">Price</div>
      <div class="ui column three wide green"></div>

      <template 
        v-for="item in items"
      >
        <div class="ui column one wide"
        v-on:remove="item.splice(index, 1)">{{ item.id }}</div>
        <div v-if="!item.editing" class="ui column six wide"
        v-on:remove="item.splice(index, 1)">{{ item.description }}</div>
        <div v-if="item.editing" class="ui column six wide"><input v-model="item.description" v-bind:placeholder="item.description" @keyup.enter="editItem(item)" type="text"></div>

        <div v-if="!item.editing" class="ui column six wide"
        v-on:remove="item.splice(index, 1)">{{ item.price }}</div>

        <div v-if="item.editing" class="ui column six wide"><input v-model="item.price" v-bind:placeholder="item.description" @keyup.enter="editItem(item)" type="text"></div>

        <div class="ui column three wide"
        v-on:remove="item.splice(index, 1)">
          <button v-if="!item.editing" class="ui yellow icon button">
            <i class="edit alternate outline icon" @click="item.editing = true"></i>
          </button>
          <button v-if="item.editing" class="ui yellow icon button">
            <i class="send alternate outline icon" @click="editItem(item)"></i>
          </button>
          <button class="ui red icon button">
            <i class="trash alternate outline icon" v-on:click="deleteItem(item)"></i>
          </button>
        </div>
      </template>

      <div class="ui column one wide"></div>
      <div class="ui column six wide"><input type="text" required v-model="newItemDescription"></div>
      <div class="ui column six wide"><input type="number" required v-model="newItemPrice"></div>
      <div class="ui column three wide">
        <button class="ui green icon button" v-on:click="addItem()">
          <i class="plus icon"></i>
        </button>
      </div>

      <div class="ui column eight wide green"><b>TOTAL</b></div>
      <div class="ui column eight wide green">{{ getTotal().toFixed(2) }}</div>
  </div>
</template>

<script>
export default {
  name: "Exam",

  methods: {
    getTotal() {
      let total = [];

      Object.entries(this.items).forEach(([key, val]) => {
        total.push(parseFloat(val.price.replace(/[^\d\.]/g, ""))); 
      });

      return total.reduce(function(total, num) {
        return total + num;
      }, 0);
    },
    addItem() {
      this.items.push({
        id: this.nextItemId++,
        description: this.newItemDescription,
        price: parseFloat(this.newItemPrice).toFixed(2),
        editing: false
      });

      this.newItemDescription = this.newItemPrice = "";

      localStorage.setItem('items', JSON.stringify(this.items))
    },
    deleteItem: function(item) {
      var index = this.items.indexOf(item);
      this.items.splice(index, 1);
      localStorage.setItem('items', JSON.stringify(this.items))
    },
    editItem: function(item) {
      item.editing = false
      // item.description = this.val
      item.price = parseFloat(item.price).toFixed(2)
      localStorage.setItem('items', JSON.stringify(this.items))
    }
  },
  data() {
    return {
      newItemPrice: "",
      newItemDescription: "",
      items: [
        { id: 0, description: "Mug", price: "100.00", editing: false },
        { id: 1, description: "Cat", price: "5000.00", editing: false }
      ],

      
    };
  },
  created() {
    this.items = JSON.parse(localStorage.getItem('items'));
    Object.entries(this.items).forEach(([key, val]) => {
      this.nextItemId =  val.id + 1
    });
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h1,
h2 {
  font-weight: normal;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
