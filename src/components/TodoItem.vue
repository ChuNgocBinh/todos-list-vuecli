<template>
  <div class="box-container">
    <input type="checkbox" v-model="toggle">
    <div class="title" :class="{'complete': toggle}">
      {{data.title}}
    </div>
    <button @click="deleteTodo">Delete</button>
  </div>
   
</template>

<script>
export default {
  name: 'HelloWorld',
  props: {
    data:{
      type:  Object,
      require: true
    },
  }, 
  data(){
    return {
      toggle: false
    }
  },
  beforeMount(){
    this.toggle = this.data.isComplete
  }, 
  watch: {
    toggle(){
      this.$emit('toggle', {
        id: this.data.id,
        isComplete: this.toggle
      })
    }
  },
  methods: {
    deleteTodo(){
      this.$emit('deleteTodo', this.data.id)
    }
  }
  
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.box-container {
  display: flex;
  border: 1px solid #ccc;
  padding: 5px;
  border-radius: 5px;
  align-items: center;
  margin-bottom: 10px;
}
.title {
  flex-grow: 1;
}
.box-container > button {
  padding: 10px 20px;
  margin-left: 10px;
  background-color: red;
  border-radius: 5px;
  color: #fff;
  border: 1px solid #ccc;
  cursor: pointer;
  outline: none;
}

.box-container > input {
  margin: 0 10px;
  cursor: pointer;
}

.complete{
  text-decoration-line: line-through;
}

</style>
