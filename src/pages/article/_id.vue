<template>
  <div style="height: 100%; padding-top: 20px; min-height: calc(100vh - 66px)">
    <article-detail v-if="id" :id="id" :data="data" can-delete></article-detail>
    <to-top></to-top>
  </div>
</template>

<script>
// import { easeout } from '~/utils/animation'
export default {
  async asyncData({ error, $api, params }) {
    try {
      const data = await $api['article/detail']({ id: params.id })
      return {
        data,
      }
    } catch (e) {
      error({ statusCode: 404 })
    }
  },
  data() {
    return {
      id: '',
      data: {
        val: '',
      },
    }
  },
  head() {
    return {
      title: this.data.title,
      meta: [
        {
          hid: 'description',
          name: 'description',
          content: this.data.abstract || this.data.val.slice(0, 100),
        },
        {
          name: 'keywords',
          content: this.data.type.split('/').join(','),
        },
      ],
    }
  },

  mounted() {
    if (!this.$route.params.id) {
      this.$router.replace('/')
    }
    this.id = this.$route.params.id
  },

  destroyed() {},
  created() {},
  methods: {},
}
</script>
<style lang="less" scoped>
.top-system {
  span {
    margin-right: 20px;
  }
}
</style>
