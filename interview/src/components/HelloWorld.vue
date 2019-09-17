<template>
  <div class="hello">
    <h3>瑜伽（1）班学生信息表</h3>
    <input placeholder="根据姓名进行搜索" id="textInput" />
    <button @click="inputInfo()"  >搜索</button>
    <button @click="setOrderType(2)">年龄升序</button>
      <button @click="setOrderType(1)">年龄降序</button>
    <table >
      <tr>
        <td>学号</td>
        <td>姓名</td>
        <td>年龄</td>
        <td>性别</td>
      </tr>
     <tr v-for="(person,index) in currentPageData" :key="index">
          <td>{{ person.num  }}</td>
          <td>{{ person.name }}</td>
          <td>{{ person.age  }}</td>
          <td>{{ person.sex  }}</td>
      </tr> 
    </table>
    <footer>
      <button @click="prevPage()">
          上一页
      </button>
      <span>第{{currentPage}}页/共{{totalPage}}页</span>
      <button @click="nextPage()">
          下一页
      </button>
    </footer>
  </div>
</template>

<script>
export default {
    //组件实例化之前 初始化数据
    beforeMount: function() {
      this.initData(50);
    },

    //自定义方法
    methods: {
      //随机算法
      randomByNum(value){
        return Math.floor(Math.random()* value)
    },

    //初始化表格数据
    initData(value){
      for(let i = 0; i < value; i++){
        this.people.push({
            num: 190110 + i,
            name: '李小华'+ i,
            age: 25+ i,
            sex: this.randomByNum(2) === 1? '男': '女'
        });
      }
    },

    // 设置当前页面数据，对数组操作的截取规则为[0~9],[10~20]...,
    // 当currentPage为1时，我们显示(0*pageSize+1)-1*pageSize，当currentPage为2时，我们显示(1*pageSize+1)-2*pageSize...
    getCurrentPageData() {
        let begin = (this.currentPage - 1) * this.pageSize;
        let end = this.currentPage * this.pageSize;
        this.currentPageData = this.people.slice(
            begin,
            end
        );
    },

    //上一页
    prevPage() {
        console.log(this.currentPage);
        if (this.currentPage == 1) {
            return false;
        } else {
            this.currentPage--;
            this.getCurrentPageData();
        }
    },

    // 下一页
    nextPage() {
        if (this.currentPage == this.totalPage) {
            return false;
        } else {
            this.currentPage++;
            this.getCurrentPageData();
        }
    },

    //排序
    setOrderType (orderType) {
        console.log("orderType",orderType)
        this.orderType = orderType
        this.filterPersons()
      },

    //获取input框中的值
    inputInfo(){
      //找到元素
      var inputDom = document.getElementById("textInput");
      //获取元素值
      this.searchName = inputDom.value;
      this.filterPersons()
    },

    //条件过滤和排序
    filterPersons () {
      // 取出相关数据
      let {searchName, people, orderType} = this
      // 过滤数组
      if(searchName.trim()) {
        this.people = people.filter(item => item.name.indexOf(searchName)!==-1)
      }
      // 排序
      if(orderType) {
        people.sort(function (p1, p2) {
          if(orderType===1) { // 降序
            return p2.age-p1.age
          } else { // 升序
            return p1.age-p2.age
          }
        })
      }
      //过滤条件后分页展示
      this.getCurrentPageData();
    }
  },

  mounted() {
    // 计算一共有几页
    this.totalPage = Math.ceil(this.people.length / this.pageSize);
    // 计算得0时设置为1
    this.totalPage = this.totalPage == 0 ? 1 : this.totalPage;
    this.getCurrentPageData();
  },
  
  data () {
    return {
      searchName: '',
      orderType: 0, // 0代表不排序, 1代表降序, 2代表升序
      people: [],
      totalPage: 1, // 统共页数，默认为1
      currentPage: 1, //当前页数 ，默认为1
      pageSize: 10, // 每页显示数量
      currentPageData: [] //当前页显示内容
      
    }
  },

}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
  table {
    margin:10px auto;
    border:1px solid lightgrey;
    border-collapse: collapse;
    width:500px;
  }
  table tr td{
    border:1px solid #dfdfdf;
    border-color: #dfdfdf;
    padding:0.5rem 0;
  }
  tr {
    height:30px
  }
  button {
    border-color: rgba(0,0,0,0);
    background: #bbb;
    border-radius: 2px;
  }
  input {
    border-color: rgba(0,0,0,.1);
    border-radius: 4px;
    height:20px;
    width: 115px;
    margin-left: 175px;
  }
  footer {
    margin-top:10px
  }
</style>
