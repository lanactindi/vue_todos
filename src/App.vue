<script>
import MemoItem from './components/memo-item.vue';

export default {
  data() {
    return {
     newAdded: false
    }
  },
  components: {
    MemoItem   
  }
}
</script>


<template>
  <MemoItem @addNew="() => newAdded = true "></MemoItem>
</template>