<!--  -->
<template>
    <div>
        <el-container>
            <el-aside class="left" :width="asideWidth">
                <!--垂直滚动组件，app.vue中提供对应全局样式【目前不需要已注释】-->
                <el-scrollbar>
                    <!--左侧菜单开始
                        default-active 默认激活菜单的index
                        collapse 是否水平折叠收起菜单
                    -->
                    <el-menu style="border-right:none;" default-active="1" :collapse="isCollapse" background-color="#7575a3" text-color="bfcbd9">
                        <!-- 左侧子菜单1 -->
                        <el-sub-menu index="1">
                            <template #title>
                                <!-- icon图标 -->
                                <el-icon><briefcase /></el-icon>
                                <span>EP组件库</span>
                            </template>
                            <!-- 菜单选项1 -->
                            <el-menu-item index="1-1" @click="goView({label:'斗破苍穹',routePath:'/main/elementplusp1'})"><el-icon><notebook /></el-icon>斗破苍穹</el-menu-item>
                            <el-menu-item index="1-2" @click="goView({label:'一人之下',routePath:'/main/elementplusp2'})"><el-icon><chat-line-square /></el-icon>一人之下</el-menu-item>
                        </el-sub-menu>
                        <!-- 左侧子菜单2 -->
                        <el-sub-menu index="2">
                            <template #title>
                                <el-icon><more-filled /></el-icon>
                                <span>图表</span>
                            </template>
                            <!-- 菜单选项2 -->
                            <el-menu-item index="2-1" @click="goView({label:'柱状图',routePath:'/main/report1'})"><el-icon><histogram /></el-icon>柱状图</el-menu-item>
                            <el-menu-item index="2-2" @click="goView({label:'折线图',routePath:'/main/report2'})"><el-icon><share /></el-icon>折线图</el-menu-item>
                            <el-menu-item index="2-3" @click="goView({label:'饼状图',routePath:'/main/report3'})"><el-icon><pie-chart /></el-icon>饼状图</el-menu-item>
                        </el-sub-menu>
                        <!-- 菜单选项3 -->
                        <el-sub-menu index="3">
                            <template #title>
                                <el-icon><grid /></el-icon>
                                <span>VUEX</span>
                            </template>
                            <el-menu-item index="3-1" @click="goView({label:'基础写法',routePath:'/main/vuexp1'})"><el-icon><collection-tag /></el-icon>基础写法</el-menu-item>
                            <el-menu-item index="3-2" @click="goView({label:'简写语法',routePath:'/main/vuexp2'})"><el-icon><share /></el-icon>简写语法</el-menu-item>
                            <el-menu-item index="3-3" @click="goView({label:'模块处理',routePath:'/main/vuexp3'})"><el-icon><copy-document /></el-icon>模块处理</el-menu-item>
                        </el-sub-menu>
                        <el-sub-menu index="4">
                            <template #title>
                                <el-icon><grid /></el-icon>
                                <span>考试项目</span>
                            </template>
                            <el-menu-item index="4-1" @click="goView({label:'开始考试',routePath:'/main/examMain'})"><el-icon><collection-tag /></el-icon>开始考试</el-menu-item>
                        </el-sub-menu>
                    </el-menu>
                    <!-- 左侧菜单结束 -->
                </el-scrollbar>
            </el-aside>
            <el-container>
                <!-- 右侧顶部区域 
                    el-header 顶栏容器
                    el-row 行
                -->
                <el-header class="head" height="45px">
                    <el-row>
                        <el-col :span="12" class="head-l">
                            <el-icon @click="isCollapse=!isCollapse" style="cursor:pointer">
                                <expand v-if="isCollapse" />
                                <fold v-else />
                            </el-icon>
                            热门国漫后台管理系统
                        </el-col>
                        <el-col :span="12" class="head-r">
                            <el-badge :value="unReadInfoTotal" :max="99" class="unReadInfo">
                                <el-link type="warning">未读消息</el-link>
                            </el-badge>
                            <!-- el-dropdown下拉菜单 
                                command	点击菜单项触发的事件回调
                            -->
                            <el-dropdown @command="handleCommand">
                                <span class="el-dropdown-link" style="cursor:pointer;">
                                    <el-icon style="top:2px;"><avatar /></el-icon>
                                    {{user_name}}
                                    <el-icon style="top:2px;" class="el-icon--right"><arrow-down-bold /></el-icon>
                                </span>
                                <template #dropdown>
                                    <!-- 下拉菜单选项 
                                        修改 详情 退出
                                    -->
                                    <el-dropdown-menu>
                                        <el-dropdown-item command="repass">修改密码</el-dropdown-item>
                                        <el-dropdown-item command="detail">账号详情</el-dropdown-item>
                                        <el-dropdown-item divided command="exit">退出登录</el-dropdown-item>
                                    </el-dropdown-menu>
                                </template>
                            </el-dropdown>
                        </el-col>
                    </el-row>
                </el-header>
                <!-- 右侧主要内容区域 -->
                 <el-main style="height:0;flex-grow:1;">
                    <el-tabs type="border-card" v-model="$store.state.tabs.activeIndex" 
                        @tab-click="tabClick"
                        @tab-remove="tabRemove">
                        <!--首页展示-->
                        <el-tab-pane label="首页" name="/main" key="-1">
                            <router-view v-slot="{ Component,route }">
                                <keep-alive v-if="route.meta.isKeepAlive">
                                    <component :is="Component" />
                                </keep-alive>
                                <component :is="Component" v-if="!route.meta.isKeepAlive" />
                            </router-view> 
                        </el-tab-pane>
                        <!--其它页展示-->
                        <el-tab-pane 
                            v-for="item in $store.state.tabs.openTab" 
                            :key="item.routePath"
                            :label="item.label"
                            :name="item.routePath"
                            :closable="true">
                            <router-view v-slot="{ Component,route }">
                                <keep-alive v-if="route.meta.isKeepAlive">
                                    <component :is="Component" />
                                </keep-alive>
                                <component :is="Component" v-if="!route.meta.isKeepAlive" />
                            </router-view>   
                        </el-tab-pane>
                    </el-tabs>    
                </el-main> 
            </el-container>
        </el-container>
    </div>
</template>

<script>
export default {
    data:function() {
        return {
            isCollapse:false,
            user_name:sessionStorage.getItem('userName')||'未登录',
            unReadlnfo:[
                {title:'请假单',count:22},
                {title:'报关单',count:44},
                {title:'送修单',count:56},
            ]
        }
    },
    computed:{
        unReadInfoTotal(){
            let num = 0
            for(let item of this.unReadlnfo){
                num += item.count
            }
            return num
        },
        //折叠菜单aside宽度变化监控
        asideWidth(){
            return this.isCollapse?'65px':'200px'
        },
    },
    methods:{
        handleCommand(command){
            console.log(command);
            switch(command){
                case 'exit':
                    this.loginOut()
                    break
                case 'repass':
                    this.$message('程序猿，该功能996模式完成中');
                    break
                case 'detail':
                    this.$message('程序猿，该功能996模式完成中');
                    break
                default:
                    break
            }
        },
        //退出登录
        loginOut(){
            //清空会话信息
            sessionStorage.clear()
            this.$router.replace({path:'/login'})
        },
        //点击左侧菜单跳转当前路由界面
        goView(params){
            //判断当前点击的路由是否已经打开，如果打开了就仅激活tab
            let isOpen=this.$store.state.tabs.openTab.findIndex((item)=>item.routePath===params.routePath)
            //如果isOpen等于-1得话就说明当前点击得路由没有打开，如果没有得调用add_tab方法将当前点击得路由(params)传入openTen数组里边
            if(isOpen === -1){
                this.$store.commit('tabs/add_tab',params)
            }
            //当前点开得哪一个路由然后调用set_active_tab方法将当前路由传入到activeInedx中打开
            this.$store.commit('tabs/set_active_tab',params.routePath)
            //跳转的路由
            this.$router.push({path:params.routePath})
        },
        //tab标签被点击时，要将当前点击的选项卡重新push一下才可以显示，否则选项卡激活了，但是路由没有被激活不会显示内容(激活路由)
        tabClick(tab){
            this.$router.push({path:tab.props.name})
        },
        //移除tab标签
        tabRemove(targetName){
            //首页不能删除，因为首页没有提供删除按钮(首页如果删除的话页面将没有内容显示，这不符合逻辑)。
            //所以此句代码可以不写
            if(targetName === '/main'){
                return
            }
            //调用VUEX中删除tab的方法
            this.$store.commit('tabs/del_tab',targetName)
            //如果当前要删除的tab(页面)处于激活状态，那么需要激活一个其他的页面
            if(this.$store.state.tabs.activeIndex === targetName){
                //openTab数组中最少要有一个对象
                if(this.$store.state.tabs.openTab && this.$store.state.tabs.openTab.length >= 1){
                    //设置当前显示页面为前一个路由(前一个页面)
                    this.$store.commit('tabs/set_active_tab',this.$store.state.tabs.openTab[this.$store.state.tabs.openTab.length - 1].routePath)
                    //跳转路由(将前一个页面的路由地址放入activeIndex里设置为默认路由)
                    this.$router.push({path:this.$store.state.tabs.activeIndex})
               }else{
                   //当删除当前路由(页面)后openTab数组中没有其他路由地址时，将激活的路径设置为默认路径(首页)
                   this.$store.commit('tabs/set_active_tab','/main')
                   //路由展示没有值时跳回首页
                   this.$router.push({path:'/main'})
               }
            }
        }
    },
    watch:{

    },
    //生命周期 - 创建完成（访问当前this实例）
    created() {

    },
    //生命周期 - 挂载完成（访问DOM元素）
    mounted() {

    }
}
</script>
<style scoped>
/* @import url(); 引入css类 */
.el-main{
    padding-top: 3px;
    padding-left: 10px;
}
.hand{
    cursor: pointer;
}
.head {
  background-color: #b3d9ff;
  padding-top: 10px;
  color: #fff;
}

.head .head-l{
    font-size: 18px;
    font-weight: bold;
}
.head .head-r{
    font-size: 18px;
    text-align: right;
}

.left {
  background-color: #7575a3;
  height: 100vh;
}

.unReadInfo{
    display: inline-block; 
    margin-right: 30px;
}
</style>