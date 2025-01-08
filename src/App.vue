<template>
  <div class="container column">
    <form class="card card-w30" @submit.prevent="submitForm">
      <div class="form-control">
        <label for="type">Тип блока</label>
        <select id="type" v-model="typeComponent">
          <option value="AppTitle">Заголовок</option>
          <option value="AppSubtitle">Подзаголовок</option>
          <option value="AppAvatar">Аватар</option>
          <option value="AppText">Текст</option>
        </select>
      </div>

      <div class="form-control">
        <label for="value">Значение</label>
        <textarea id="value" rows="3" v-model="text"></textarea>
      </div>

      <button class="btn primary" :disabled="text.length === 0">
        Добавить
      </button>
    </form>

    <div class="card card-w70" v-if="components.length !== 0">
      <component
        v-for="component in components"
        :key="component.type"
        :is="component.type"
        v-bind="component.props"
      ></component>
    </div>
    <div v-else class="card card-w70">
      <h3 class="text-center">Добавьте первый блок, чтобы увидеть результат</h3>
    </div>
  </div>
  <app-loader v-if="isLoading"></app-loader>

  <app-comments v-else @load="loadComments" :comments="listComments"></app-comments>
</template>

<script>
import AppAvatar from './AppAvatar.vue'
import AppLoader from './AppLoader.vue'
import AppSubtitle from './AppSubtitle.vue'
import AppText from './AppText.vue'
import AppTitle from './AppTitle.vue'
import AppComments from './AppComments.vue'

export default {
  data () {
    return {
      typeComponent: 'AppTitle',
      text: '',
      isLoading: false,
      components: [],
      listComments: []
    }
  },
  components: {
    AppTitle,
    AppSubtitle,
    AppAvatar,
    AppText,
    AppLoader,
    AppComments
  },
  methods: {
    submitForm () {
      const type = this.typeComponent
      let props = {}

      if (type === 'AppTitle') {
        props = { title: this.text }
      } else if (type === 'AppSubtitle') {
        props = { subtitle: this.text }
      } else if (type === 'AppText') {
        props = { text: this.text }
      } else if (type === 'AppAvatar') {
        props = { src: this.text }
      }

      this.components.push({ type, props })
      console.log('Весь объект', this.components)

      this.text = ''
    },
    loadComments () {
      this.isLoading = true
      setTimeout(async () => {
        const response = await fetch('https://jsonplaceholder.typicode.com/comments?_limit=42')

        console.log(response)

        this.listComments = await response.json()
        this.isLoading = false
      }, 1500)

      console.log(this.listComments)
    }
  }
}
</script>

<style>
.avatar {
  display: flex;
  justify-content: center;
}

.avatar img {
  width: 150px;
  height: auto;
  border-radius: 50%;
}

.text-center {
  text-align: center;
}
</style>
