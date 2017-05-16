<template>
<div>
    <mu-appbar title="Title" id="cnode_top" style="height:50px;background-color: rgba(153,102,255,0.9);">
        
        <!--左边菜单-->
        <span class="myMenu" @click="changeMenu()" style="position:fixed;left:10px;top:14px;"></span>
        <mu-drawer :open="open" :docked="docked" @close="changeMenu()">
        <mu-list @itemClick="docked ? '' : changeMenu()" style="padding-top:0px">
            <p style="background:rgba(153, 102, 255, 0.9);height:50px;color:white;text-align:center;line-height:50px">
                <span style="font-size: 30px;position: fixed;left: 0px;width:50px" @click="open = false">&times;</span>
                CNode
            </p>
            <i class="logo"></i>
            
            <div style="font-size:16px;padding-left:16px;border-bottom: 1px solid #ddd">省流量
                <mu-switch v-model="toggle" @input="economy()"  class="demo-switch" style="margin-left:120px"/>
            </div>
            <mu-list-item title="关于作者" style="border-bottom: 1px solid #ddd" href="https://github.com/stjw7098"/>
            <mu-list-item title="关于CNode社区" style="border-bottom: 1px solid #ddd" href="https://cnodejs.org/"/>
            <mu-list-item title="当前版本  1.0" style="border-bottom: 1px solid #ddd"/>
            <mu-list-item v-if="docked" @click.native="open = false" title="关闭"/>
        </mu-list>
        </mu-drawer>

        <!--顶部导航-->
        <mu-tabs :value="activeTab" @change="handleTabChange" style="height:50px;background-color: rgba(255,255,255,0);font-family:bold;margin-left:20px">
            <!--<a href="#/index/list/:">-->
            
            <mu-tab value="tab1" title="全部" @click="updateTab(0)" href="javascript:scroll(0,0)"/>
            <mu-tab value="tab2" title="精华" @click="updateTab(1)" href="javascript:scroll(0,0)"/>
            <mu-tab value="tab3" title="分享" @click="updateTab(2)" href="javascript:scroll(0,0)"/>
            <mu-tab value="tab4" title="问答" @click="updateTab(3)" href="javascript:scroll(0,0)"/>
            <mu-tab value="tab5" title="招聘" @click="updateTab(4)" href="javascript:scroll(0,0)"/>
        </mu-tabs>
        <mu-icon-menu icon="more_vert" slot="right">
            <mu-menu-item class="menuItem1" title="登录" @click="login=true" style="margin-left:10px;padding-left:10px;"/>
            <mu-menu-item class="menuItem2" title="关于" href="#/about" style="margin-left:10px;padding-left:10px;font-weight:normal"/>
            
        </mu-icon-menu>
  </mu-appbar>
  <!--遮罩-->
  <div class="alert-matte" v-show="login" @click="login=false"></div>
  <!--//登录框-->
  <div class="login-dialog alert" v-show="login"> 
      <input class="count" type="text" placeholder="AccessToken"> 
      <button class="lbtn">登 录</button>  
  </div>
  <!--懒加载-->
  <mu-circular-progress class="loadmore" :size="40" v-show="flag"/>
  <router-view v-show="!flag"></router-view>
</div>
</template>

<script>
var list=require("./list.vue");
export default {
  data () {
    return {
      activeTab: 'tab1',
      id:"",
      atBottom:false,
      currentTab:'',
      login:false,

      open: false,
      docked: true,
      toggle:false
    }
  },
  components: {
      wlist:list
  },
  computed: {
      flag:function(){
            console.log(this.$store.getters.getFlag)
            return this.$store.getters.getFlag;
        },
  },

  methods: {
    handleTabChange (val) {
      this.activeTab = val
    },
    //改变tab切换
    updateTab:function(tab){
        this.currentTab=tab;

        this.atBottom=true;
        this.$store.commit('setChange',{
            tab:tab,
            page:1,
            flag:true
        });
        //  this.atBottom=false;
    },
    //控制左边菜单的开关
    changeMenu (flag) {
      this.open = !this.open
    //   this.docked = !flag
    },
    economy(){
        this.$store.commit('setEconomy',this.toggle);
    }

  },
  mounted:function(){
      $(".loadmore").css('left',(window.innerWidth-$(".loadmore").width())/2)
  }
}
</script>
<style scoped lang="scss">
    .count{
        outline: none;
    }
    .logo{
        background:url('../img/cnode.jpg') no-repeat;
        background-size: 200px 200px;
        width: 200px;
        height: 200px;
        display: inline-block;
        margin-left: 27px;
        margin-top: 15px;
        margin-bottom: 15px;
    }
    .myMenu{
        background:url('../img/menu.png') no-repeat;
        background-size: 24px 24px;
        display: inline-block;
        width: 24px;
        height: 24px;
    }
    .menuItem1{
        background:url('../img/login.png') no-repeat left center;
        background-size: 20px 20px;
    }
    .menuItem2{
        background:url('../img/about.png') no-repeat left center;
        background-size: 20px 20px;
    }
    #cnode_top{
        position: fixed;
        top:0;
    }
    .loadmore{
        margin-top: 80px;
    }

    .alert {
        width: 280px;
        left: 50%;
        margin-left: -140px;
        margin-top: 50px;
        position: fixed;
        padding: 20px;
        background: #fff;
        border-radius: 5px;
        box-sizing: border-box;
        box-shadow: 0 5px 15px #555;
        text-align: center;
        color: #f70;
        z-index: 20000;
    }

    .login-dialog input[type=text] {
        border: 2px solid rgba(153, 102, 255, 0.9);
        border-radius: 5px;
        color: rgba(153, 102, 255, 0.9);
        -webkit-transition: all .2s ease;
        transition: all .2s ease;
    }

    .login-dialog input {
        margin-bottom: 10px;
        width: 100%;
        line-height: 30px;
        padding: 5px;
        display: block;
    }

    .btn, .lbtn {
        margin: 10px auto;
        padding: 0;
        display: block;
        background-color: rgba(153, 102, 255, 0.9);
        color: #fff;
        font-size: 14px;
    }

    .lbtn {
        width: 100%;
        height: 40px;
        line-height: 40px;
        border: none;
        border-radius: 5px;
    }

    .alert-matte {
        width: 100%;
        height: 100%;
        position: fixed;
        background-color: #000;
        opacity: .7;
        z-index: 15000;
        top:0px;
    }
   
</style>