<template>
  <div class="container">
    <p>
      <button class="btn primary" @click="$emit('load')">Загрузить комментарии</button>
    </p>
    <div class="card" v-if="comments.length !== 0">
      <h2>Комментарии</h2>
      <ul class="list">
        <li class="list-item" v-for="item in comments" :key="item">
          <div>
            <p><strong>{{ item.email }}</strong></p>
            <small
              >{{ item.body }}</small
            >
          </div>
        </li>
      </ul>
    </div>
  </div>
</template>

<script>
export default {
  emits: ['load'],
  props: {
    comments: Array
  }
}
</script>

<style>

</style>
