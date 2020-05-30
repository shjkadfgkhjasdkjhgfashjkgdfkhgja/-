<template>
    <div class="CommentComponent">
        <div class="up">
            <textarea type="text" placeholder="请发表你的评论" ref="inputComment"> </textarea> 
            <van-button type="primary" class="submit" @click="pushComment">发表评论</van-button>
        </div>
  
        <div class="comment">
            
             <div class="commentContent" v-for="(item,index) in comment.slice(0,limitNum)" :key="item.id"> 
                 <p>第{{comment.length - index}}楼说：</p> 
                 <p>发布时间:{{item.time}}</p>
                 <div class="CC">{{item.content}}</div>
            
    
             </div>
               
        <van-button type="info" class="clickMore" @click="showMore">点击加载更多</van-button>
         </div>
    </div>
</template>
<script>
import Vue from 'vue';
import { Button,Dialog } from 'vant';

Vue.use(Button);

export default ({
    props:{
       "commentData":{
         type:Array
       }
    },
    data(){
        return {
            comment:'',
            limitNum:3
           
        }
    },
    methods:{
       getComment(){
           let arr = [];

           for(let i =  this.commentData.length; i >= 0; i--){
                  
                    arr[this.commentData.length - 1 - i] = this.commentData[i]
               
           }
                      
                      this.comment =  this.commentData; 
                     
       },
       pushComment(){
             if(!this.$refs.inputComment.value.length){
               Dialog({ message: '评论不能为空' });
                 return;
             }
             let nowComment = this.$refs.inputComment.value;
             let myDate = new Date()
             this.commentData.unshift(
                 {
                     id: this.commentData.length + 1,
                     time: myDate.getFullYear()+'年' + (myDate.getMonth() + 1) + '月'+ myDate.getDate()+'日'+ myDate.getHours()+'时'+ myDate.getMinutes()+'分',
                     content:nowComment
                 }
             )
             this.$refs.inputComment.value = '';
       },
       showMore(){
          this.limitNum += 3;
       }
    },

    mounted(){
     
        this.getComment();
    }
})
</script>  
<style lang="less" scoped>
 .CommentComponent{
      padding-bottom: 7.2rem;
      .clickMore{       
          width: 43%;
          height: 4rem;
          line-height: 4rem;
          margin: 1rem auto;
          display: block;
           font-size: 2.1rem;
      }
     .up{
         text-align: right;
        textarea{
           
            width: 90%;
            height: 10rem;
            margin: 0 auto;
            display: block;
            resize:none;        
             font-size: 1.8rem;
        }
          .submit{
           margin-top:1rem;
          width: 90%;
          height: 4rem;
          line-height: 4rem;
          margin: 0.3rem auto;
          display: block;
           font-size: 2.1rem;
        }
     }
       
        .comment{
            
         .commentContent{
             width: 90%;
             display: block;
             margin: 0 auto;
             font-size: 1.8rem;
             margin-top:1.5rem;
             p{
              font-size: 1.6rem;
             }
             .CC{
 
              padding-top:1rem;
              word-break:break-all;
             }
          } 
        }
 }
 
</style>