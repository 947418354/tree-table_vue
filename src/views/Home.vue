<template>
  <div class="home">
    <div>我是主页</div>
    <tree-table :cols="cols" :data="data"></tree-table>
  </div>
</template>

<script>
import treeTable from '../components/treeTable'
export default {
  name: 'home',
  components: {
    treeTable
  },
  data(){
    return {
      data: {},
      cols: [
        {
          type: 'normal',
          name: '节点名',
          width: '200px'
        },
        {
          type: 'option',
          name: '操作',
          width: '50px'
        }
      ]
    }
  },
  mounted(){
    this.$axios.get('http://localhost:8080/tree.json')
    .then((res)=>{
      this.data = res.data
    })
  }
}
</script>
