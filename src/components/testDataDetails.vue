<template>
  <div class="counter">
    <p>{{ value1 }}
    <button @click="$emit('removeItem', id)">Remove</button></p>
  </div>
</template>

<script>
export default {
  name: 'testDataDetails',
  props: {
    id: Number,
    value1: String,
    value2: Number,
    value3: String
  }
}
</script>

