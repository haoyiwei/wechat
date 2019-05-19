<template>
  <div>
    <!-- 搜索条 -->
    <search-bar></search-bar>
    <!-- 菜单和内容 -->
    <div class="content">
      <div class="left">
        <div
          @click='changeBrand(item.cat_id)'
          :class='{active: indexid === item.cat_id}'
          :key='item.cat_id'
          v-for='item in cate'
          class="menu-item"
        >
          {{item.cat_name}}
        </div>
      </div>
      <div class="right">
        <div
          :key='item1.cat_id'
          v-for='item1 in rightdata'
          class="brand-item"
        >
          <div class="brand-title">{{item1.cat_name}}</div>
          <div class="brand-list">
            <div
              :key='i'
              v-for='(img, i) in item1.children'
              class="brand"
            >
              <img
                :src="img.cat_icon"
                mode="aspectFill"
              >
              <p>{{img.cat_name}}</p>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import request from '@/utils/com'
import searchBar from '../../components/searchBar'
export default {
  components: {
    'search-bar': searchBar
  },
  created() {
    this.cateData()
  },
  data() {
    return {
      indexid:1,
      cate: []
    }
  },
  computed: {
    rightdata () {
     let rd = this.cate.filter(item=>{
        return item.cat_id === this.indexid
      })
      let ret = rd[0].children
      return ret
    }
  },
  methods: {
    async cateData() {
      // 调用接口获取分类数据
      let ret = await request('categories')
      this.cate = ret.data.message
    },
      changeBrand (index) {
        this.indexid = index
  }
  },

}
</script>

<style scoped lang='scss'>
@import "main.scss";
</style>