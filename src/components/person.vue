<template>
  <div style="padding:20px;" id="app">
    <div class="panel panel-primary">
      <div class="panel-heading">用户标签</div>
      <table class="table table-bordered table-striped text-center">
        <thead>
        <tr>
          <th>序号</th>
          <th>姓名</th>
          <th>性别</th>
          <th>年龄</th>
          <th>艺术兴趣值</th>
          <th>宗教兴趣值</th>
          <th>社会科学兴趣值</th>
          <th>自然科学兴趣值</th>
          <th>文学兴趣值</th>
          <th>历史地理兴趣值</th>
          <th>应用科学兴趣值</th>
        </tr>
        </thead>
        <tbody>
        <tr v-for ="(lable,index) in latestLables" v-bind:key="index">
          <td>{{index+1}}</td>
          <td>{{lable.name}}</td>
          <td>{{lable.gender}}</td>
          <td>{{lable.age}}</td>
          <td>{{lable.art_value}}</td>
          <td>{{lable.religion_value}}</td>
          <td>{{lable.socialSci_value}}</td>
          <td>{{lable.Sci_value}}</td>
          <td>{{lable.literature_value}}</td>
          <td>{{lable.hisgeo_value}}</td>
          <td>{{lable.appliSci_value}}</td>
        </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>

<script>
  export default {
    name: 'hello',
    data () {
      return {
        lables: [
          {'name': '赵康', 'gender':'男','age':'21','art': '25','religion':'0','socialSci':'50','Sci':'10','literature':'80','hisgeo':'10','appliSci':'120'},
          {'name': '钱由军', 'gender':'男','age':'22','art': '5','religion':'0','socialSci':'50','Sci':'10','literature':'120','hisgeo':'70','appliSci':'30'},
          {'name': '孙为名', 'gender':'男','age':'22','art': '10','religion':'0','socialSci':'40','Sci':'10','literature':'40','hisgeo':'40','appliSci':'50'},
        ]
      }
    },
    created() {
      if(this.$store.state.usersInfo.length==0){
        let that=this
        $.ajax(
        'http://localhost:3001',
        {
        
          type:"post",            //get不能传输数据体？这里只能用post
          data:{
            need:'usersInfo',
          },            //client向server传输的数据，以query的形式添加到url上
          dataType:"json",    //client接收的数据类型
          success:function(data){
            //alert('数据返回成功了')
            var interestValue=new Array(0,0,0,0,0,0,0);
            console.log(data)
            that.$store.commit("setUsersInfo",data)      //这里store.commit方法只能放在methods中,且运行不能放在created hook中
            $.each(data,function(index,info){
              interestValue[0]+=info["art_value"]
              interestValue[1]+=info["religion_value"]
              interestValue[2]+=info["socialSci_value"]
              interestValue[3]+=info["Sci_value"]
              interestValue[4]+=info["literature_value"]
              interestValue[5]+=info["hisgeo_value"]
              interestValue[6]+=info["appliSci_value"]
            })
            console.log('interestvalue如下')
            console.log(interestValue)
            //this.$store.commit('setInterestValue',interestValue)
            
           
            console.log(that.$store.state.usersInfo)
            that.lables=that.$store.getters.usersInfo
          }
        
        })
        console.log('ajax方法发生了')
      }
    },
    mounted(){
      
    },
    computed:{
      latestLables:function(){
        return this.lables
      }
    }
  }
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
  h1, h2 {
    font-weight: normal;
  }

  ul {
    list-style-type: none;
    padding: 0;
  }

  li {
    display: inline-block;
    margin: 0 10px;
  }

  a {
    color: #42b983;
  }
  tr,th{
    text-align:center;
  }
</style>
