<template lang="pug">
  div.container
    div.news-wraper
      news(:newss="newss" flag="news-list")
</template>


<script>
  import axios from '~plugins/axios'
  import News from '~components/news.vue'
  let pagesize = 10
  export default {
    name: 'home',
    data () {
      return {
        pagesize: pagesize,
        newss: [],
        pagetotal: 0
      }
    },
    computed: {
      session () {
        return this.$store.state.session
      }
    },
    head () {
      return {
        title: `前端情报局`,
        meta: [
          { hid: 'description', name: 'description', content: '最新前端资讯和情报' }
        ]
      }
    },
    components: {
      News
    },
    methods: {
      list: async function () {
        let page = 1
        let res = await axios().get(`news?mem=${this.$route.params.id}`, {
          params: {
            limit: pagesize,
            skip: pagesize * (page - 1)
          }
        })

        let newss = res.data.items
        newss.forEach(item => {
          item.isShowCom = false
        })
        this.newss = newss
        this.pagetotal = res.data.count
      }
    },
    created () {
      this.list()
    }
  }
</script>


<style lang="scss" scoped>
  .container {
    max-width: 800px;
  }
  .card {
    display: block;
    background-color: #FFF;
    padding: 30px;
    overflow: hidden;
    margin-bottom: 20px;
  }

  .new-bar {
    margin-bottom: 10px;
  }

  .pub-news {
    padding: 50px;
    background-color: #FFF;
    position: fixed;
    z-index: 80;
    width: 100%;
    max-width: 500px;
    left: 0;
    right: 0;
    margin: auto;
    top: 60px;
    border-bottom: #EEE 1px solid;
    box-shadow: 1px 1px 1px rgba(238, 238, 238, 0.54);
    border-left: #FAFAFA 1px solid;

    .meditor {
      min-height: 100px;
    }

    .btn-wraper {
      margin-top: 10px;
      .btn {
        padding: 0.7rem 2rem;
        width: 100%;
      }
    }
      
  }
</style>
