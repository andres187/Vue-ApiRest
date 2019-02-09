<template>
  <div class="post__container">
    <div class="card">
      <div class="card-body">
        <h5 class="card-text">{{description}}</h5>
        <p>{{author}}</p>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'post',
  props: {
    description: {
      type: String,
      default: '',
    },
    author: {
      type: String,
      required: true
    },
  },
};
</script>

<style>
    .post__container{
        margin: 10px;
    }
</style>
