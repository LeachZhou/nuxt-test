<template>
  <div class="container">
    <div class="item" v-for="(item, index) in list" :key="index">
      <div
        v-if="item.item_type === 2"
        class="title"
        @click="linkPost(item.item_type, item.item_info.article_id)"
      >
        {{ item.item_info.article_info.title }}
      </div>
      <div
        v-if="item.item_type === 14"
        class="title"
        @click="linkPost(item.item_type, item.item_info.url)"
      >
        {{ item.item_info.title }}
      </div>
    </div>
  </div>
</template>

<script lang="ts">
export default {
  name: 'Template',
  async asyncData(e: object) {
    const { $axios } = e
    const { data } = await $axios.post(
      'https://api.juejin.cn/recommend_api/v1/article/recommend_all_feed',
      {
        id_type: 2,
        client_type: 2608,
        sort_type: 200,
        cursor: '0',
        limit: 20,
      }
    )

    return {
      list: data.data,
    }
  },
  methods: {
    linkPost(type: number, value: string) {
      switch (+type) {
        case 2:
          window.open(`https://juejin.cn/post/${value}`)
          break
        case 14:
          window.open(value)
          break
      }
    },
  },
}
</script>

<style lang="scss" scoped>
.container {
  margin: 0 auto;
  min-height: 100vh;
  width: 750px;
}
.item {
  padding: 10px 0;
  .title {
    color: #3c5761;
    font-weight: bold;
    font-size: 14px;
    cursor: pointer;
  }
}
</style>
