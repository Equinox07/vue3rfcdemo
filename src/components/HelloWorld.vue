<template>
  <div class="hello">
    <p>
      For a guide and recipes on how to configure / customize this project,<br>
      check out the
    </p>
     <div v-if="isPending" >Loading....</div>
    <div v-else-if="data" >{{data}}</div>
    <div v-else-if="error" >Something went wrong: {{error.message}}</div>
  </div>
</template>

<script>
// import { ref } from "@vue/composition-api";
import { ref, watchEffect } from "vue";

function usePost(getId) {
  return useFetch(() => `https://jsonplaceholder.typicode.com/posts/${getId()}`)
}

function useFetch(getUrl) {
  const data = ref(null)
  const error = ref(null)
  const isPending = ref(true)

  watchEffect(() => {
    isPending.value = true
    data.value = null
    error.value = null
    fetch(getUrl())
    .then(res   => res.json())
    .then(_data => {
        data.value = _data
        isPending.value = false
      }).catch(err => {
        error.value = err
        isPending.value = false
      })
  })

  return {
      data,
      error,
      isPending
    }

}
export default {
  name: 'HelloWorld',
  props: ['id'],
  setup(props){
    const {data, error, isPending} = usePost(() => props.id )
    return {
      data,
      error,
      isPending
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
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
