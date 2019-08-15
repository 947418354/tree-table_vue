<template>
  <div class="my-treetable-container">
    <div>我是数表组件</div>
    <table class="table-head">
        <tr>
            <th v-for="(col,i) of cols" :key="i" :style="{width: col.width}">{{col.name}}</th>
        </tr>
    </table>
    <table class="table-content">
        <tr v-for="(item,i) of dataArr" :key="i">
            <td v-for="(col,i) of cols" :key="i" :style="{width: col.width}">
                <div v-if="col.type === 'normal'">
                    <i class="space" v-for="(it,i) of item.level" :key="i"></i>
                    <i :class="item.expanded? 'des':'ten'" v-if="item.children && item.children.length > 0" @click="handleCollapse(item)"></i>
                    <i class="space" v-else></i>
                    {{item.name}}
                </div>
                <div v-if="col.type === 'option'">
                    <button @click="handleDel(item, data)">删除</button>
                </div>
            </td>
        </tr>
    </table>
  </div>
</template>

<script>
  export default {
    name:'',
    props:{
        cols: {
            type: Array,
        },
        data:{
            type: null,
        }
    },
    data () {
      return {
          dataArr: [],
      };
    },

    components: {},

    computed: {},

    beforeMount() {},

    mounted() {
        // this.initData(this.data)
        // this.dataArr.push({name:'dddd'})
        // console.log(this.dataArr)
    },
    watch:{
        data: {
            handler:function(n){
                this.dataArr = [];
                this.initData(n)
            },
            deep: true,
        }
    },
    methods: {
        initData(data){
            if(Object.prototype.toString.call(data) === '[object Object]'){
                if(data.children){
                    for(let i=0;i<data.children.length;i++){
                        this.dataArr.push(data.children[i])
                        if(data.children[i].expanded){
                            this.initData(data.children[i])
                        }
                    }
                }
            }
        },
        handleCollapse(item){
            console.log(item)
            item.expanded = !item.expanded;
        },
        handleDel(item, data){
            if(Object.prototype.toString.call(data) === '[object Object]'){
                if(data.children){
                    for(let i=0;i<data.children.length;i++){
                        if(data.children[i] === item){
                            data.children.splice(i,1)
                            return
                        }
                        this.handleDel(item, data.children[i])
                    }
                }
            }
        }
    },

  }

</script>
<style lang='less'>
*{
    box-sizing: border-box;
}
    .my-treetable-container{
        .table-head{
            border-spacing: 0;
            border-collapse: collapse;
            th{
                border:1px solid #000;
            }
        }
        .table-content {
            border-spacing: 0;
            border-collapse: collapse;
            td {
                border:1px solid #000;
                text-align: left;
                .space::before {
                    content: '';
                    display: inline-block;
                    width: 1rem;
                }
                .ten::before{
                    content: '+';
                    display: inline-block;
                    width: 1rem;
                    height: 1rem
                }
                .des::before{
                    content: '-';
                    display: inline-block;
                    width: 1rem;
                    height: 1rem
                }
            }
        }
    }
</style>