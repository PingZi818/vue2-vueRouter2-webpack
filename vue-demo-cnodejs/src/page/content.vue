<template>
  <div>
    <myHeader></myHeader>
    <Uediter :value="ueditor.value" :config="ueditor.config" ref="ue"></Uediter >
    <input type="button" value="显示编辑器内容（从控制台查看）" @click="returnContent">
    <img src="static/image/beauty.png" alt="刘亦菲">
    <h2 v-text="dat.title"></h2>
    <p>作者：{{dat.author.loginname}}　　发表于：{{$utils.goodTime(dat.create_at)}}</p>
    <hr>
    <article v-html="dat.content"></article>
    <h3>网友回复：</h3>
    <ul>
      <li v-for="i in dat.replies">
        <p>评论者：{{i.author.loginname}}　　评论于：{{$utils.goodTime(i.create_at)}}</p>
        <article v-html="i.content"></article>
      </li>
    </ul>
    <myFooter></myFooter>
  </div>
</template>
<script>
import myHeader from '../components/header.vue'
import Uediter from '../components/ueditor.vue'
import myFooter from '../components/footer.vue'
export default {
  components: { myHeader, myFooter,Uediter },
  data () {
    return {
      id: this.$route.params.id,
      dat: {},
      data: {
        content: ''
      },
      ueditor: {
        value: '编辑器默认文字',
        config: {}
      }
    }
  },
  created () {
    this.getData()
  },
  methods: {
    getData () {
      this.$api.get('topic/' + this.id, null, r => {
        this.dat = r.data
      })
    },
    returnContent () {
      this.data.content = this.$refs.ue.getUEContent()
      console.log(this.data.content)
    }
  }
}
</script>
