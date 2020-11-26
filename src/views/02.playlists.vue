<template>
  <div class="playlists-container">
    <!-- 同步 -->
    <div class="top-card">
      <div class="icon-wrap">
        <img :src="topList.coverImgUrl" alt="" />
      </div>
      <div class="content-wrap">
        <div class="tag">精品歌单</div>
        <div class="title">
         {{topList.copywriter}}
        </div>
        <div class="info">
        {{topList.description}}
        </div>
      </div>
      <img src="../assets/listCover.jpg" alt="" class="bg" />
      <div class="bg-mask"></div>
    </div>
    <div class="tab-container">
      <!-- tab栏 顶部 -->
      <div class="tab-bar">
        <span class="item" :class="{active: Tag=='全部'}" @click="Tag = '全部'">全部</span>
        <span class="item" :class="{active: Tag=='欧美'}" @click="Tag = '欧美'">欧美</span>
        <span class="item" :class="{active: Tag=='华语'}" @click="Tag = '华语'">华语</span>
        <span class="item" :class="{active: Tag=='流行'}" @click="Tag = '流行'">流行</span>
        <span class="item" :class="{active: Tag=='说唱'}" @click="Tag = '说唱'">说唱</span>
        <span class="item" :class="{active: Tag=='摇滚'}" @click="Tag = '摇滚'">摇滚</span>
        <span class="item" :class="{active: Tag=='民谣'}" @click="Tag = '民谣'">民谣</span>
        <span class="item" :class="{active: Tag=='电子'}" @click="Tag = '电子'">电子</span>
        <span class="item" :class="{active: Tag=='轻音'}" @click="Tag = '轻音乐'">轻音乐</span>
        <span class="item" :class="{active: Tag=='影视'}" @click="Tag = '影视原声'">影视原声</span>
        <span class="item" :class="{active: Tag=='ACG'}"  @click="Tag = 'ACG'">ACG</span>
        <span class="item" :class="{active: Tag=='怀旧'}" @click="Tag = '怀旧'">怀旧</span>
        <span class="item" :class="{active: Tag=='治愈'}" @click="Tag = '治愈'">治愈</span>
        <span class="item" :class="{active: Tag=='旅行'}" @click="Tag = '旅行'">旅行</span>
      </div>
      <!-- tab的内容区域 -->
      <div class="tab-content">
        <div class="items">
          <div class="item" v-for="(item,index) in list"  :key="index">
            <div class="img-wrap">
              <div class="num-wrap">
                播放量:
                <span class="num">{{item.playCount}}</span>
              </div>
              <img :src="item.coverImgUrl" alt="" />
              <span class="iconfont icon-play"></span>
            </div>
            <p class="name">{{item.name}}}</p>
          </div>
         
        </div>
      </div>
    </div>
    <!-- 分页器 
    total 数据总条数
    current-pag 当前页
    page-size 每页多少数据
    @current-change 页码改变事件
    
    
    
    -->
    <el-pagination
      @current-change="handleCurrentChange"
      background
      layout="prev, pager, next"
      :total="total"
      :current-page="page"
      :page-size="10"
    >
    </el-pagination>
  </div>
</template>

<script>
import axios from "axios"
export default {
  name: 'recommend',
  data() {
    return {
      // 总条数
      total:0,
      // 页码
      page:1,
      topList: {},
      list: [],
      Tag: "全部"

    };
  },
  //侦听器 当Tag的值发生改变时即调用这个函数
  watch: {
 Tag(){
  //  console.log(this.Tag);
  this.page = 1 ;
  //精品歌单
    this.topData();
    //歌单列表
    this.listData();
    

 }
  },
  methods: {
    handleCurrentChange(val) {
      // console.log(`当前页: ${val}`);
      this.page = val;
      this.listData();
      
      
    },
    //抽取的方法1 顶部的数据
      topData(){
        axios({
      url: "https://autumnfish.cn/top/playlist/highquality",
      methods: "get",
      params: {
        limit: 1,
        cat: this.Tag
      }
    }).then(res=>{
      // console.log(res);
      this.topList = res.data.playlists[0];
    })
      },
      //抽取的方法2 列表数据
      listData(){
         axios({
      url: "https://autumnfish.cn/top/playlist/",
      methods: "get",
      params: {
        limit: 10,
        
        offset: (this.page - 1) *10, //起始的值 = （页码 - 1） * 每页数据的条数
        cat: this.Tag
      }
    }).then(res=>{
      // console.log(res);
      this.list = res.data.playlists;
      this.total = res.data.total;
    })
      }
  },
  created() {
    //精品歌单
   this.topData();
    //歌单列表
    this.listData();
  }

};
</script>

<style >

</style>
