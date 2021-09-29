<template>
  <div>
    <el-input v-model="num" style="width:150px"></el-input>
    <label>内</label>
    <el-input v-model="target" style="width:150px"></el-input>
    <el-button @click="select()">{{this.num}}内{{target}}的个数：{{this.count}}</el-button>
    <el-button @click="paop(data)">泡泡</el-button>
    <el-button @click="two(data)">二分</el-button>
    <el-button @click="retun(data)">归并</el-button>
  </div>
</template>

<style>
  .demo-table-expand {
    font-size: 0;
  }
  .demo-table-expand label {
    width: 90px;
    color: #99a9bf;
  }
  .demo-table-expand .el-form-item {
    margin-right: 0;
    margin-bottom: 0;
    width: 50%;
  }
</style>

<script>
  export default {
    data() {
      return {
        num:6,
        target:2,
        data:[14,13,12,11,10,9,8,7,6,5,4,3,2,1,0],
        // int: 2020,
        count: 0,
        char:2,
        time:"",
        m:0,//分
        s:0,//秒
        ms:0,//毫秒
        startTime:0,//开始时间
        endTime:0//结束时间
      }
    },
    methods:{
      //计算运行时间
      runTime(startTime,endTime){
        var a = 0;
        a = endTime - startTime;
        this.ms = a % 1000;
        this.s = (a - this.ms) / 1000
        this.m = Math.floor(this.s / 60)
        this.time = this.m + "m:" + this.s % 60 + "s:" +this.ms + "ms";
        console.log("总时间：ms" + a)
      },

      // 查询字符
      select(){
        this.startTime =  new Date().getTime()
        console.log("开始时间：" + this.startTime)
        this.count=0
        let a=''
        let s =[]
        var c = this.char; // 要计算的字符
        var regex = new RegExp(this.target, 'g'); // 使用g表示整个bai字符串都要匹配du
        let count = this.num;
        let t;
        for(var i=0; i <= this.num ; i++ ){
        // //每个数字进行比较；
          var r = i.toString().match(regex)
          if(r){
            this.count += r.length;
          }
        }
        
        // let j = 1;
        // let l = 0;
        // let h = 0;
        // let c1 = 0;
        // while(Math.floor(this.num/j) != 0){
        //   l = this.num-(this.num/j)*j
        //   c1 = this.num/j;
        //   h = this.num/(j*10);
        //   switch(c1){
        //     case 0: this.count+=h*j;
        //         break;
        //     case 1:this.count+=h*j+l+1;
        //         break;
        //     default:this.count+=(h+1)*j;
        //         break;
        //   }
        //   j*=10;
        // }
        this.endTime = new Date().getTime()
        console.log("结束时间：" + this.endTime)
        this.runTime(this.startTime,this.endTime)
        console.log("运行时间：" + this.time)
        alert( count + "中：" + this.target+ " 的数量为 " + this.count)
        console.log( count + "中：" + this.target+ " 的数量为 " + this.count);
      },
// 冒泡
      paop(data){
        this.count=0
        let d=JSON.parse(JSON.stringify(data))
        // let d=this.data
        let a=''
        console.log(this.data)
        for(let j=1;j<d.length;j++){
          for(let i=0;i<d.length-j;i++){
            //  console.log('i',i,'j',j,'d.length-j',d.length-j)
            if(d[i]>d[i+1]){
              a=d[i+1]
              d[i+1]=d[i]
              d[i]=a
            }
          this.count++
          // console.log('进行第', this.count, '次:',d)
          // console.log('===',d)
          }
        }
        console.log('===',d,'排序次数:',this.count)
        return d
      },
// 二分
      two(data){
        this.count=0
        // let d=[0,1,2,3,4,5,6,7,8,9,10,11,12,13,14]
        // let d=[9,10,11,12,0,1,2,3,4,5,6,7,8,13,14]
        let d=this.data
        // let d=JSON.parse(JSON.stringify(data))//JSON.stringify(data)对象转json字符串，JSON.parse()字符串转对象
        d=this.paop(d)//二分法需要先有序
        // let n=this.data.length
        console.log('d:',d)
        let left=0
        let right=d.length-1
        let n=Math.floor((left+right)/2)
        // console.log('n:::',n,'dn:::',d[n])
        let a=this.num
        let flg=true
        while(flg===true){
          if(d[n]==a){
            flg=false
          }
          // console.log(d[n],'==',a)
          // console.log(flg,':',d[n]==a)
          if(d[n]>a){
            right=n-1
          }else if(d[n]<a){
           left=n+1
          }
          if(left<=right){
           n=Math.floor((left+right)/2)
          }else if(left>right){
            console.log('不存在元素',a)
            break
          }
          // console.log(n)
          this.count++
        }
        console.log('=====元素',this.num,'在下标:',n,'处，d[',n,']值：',d[n],'查询次数：',this.count)
      },
      // 归并
      retun(data){
        this.count=0
        // let d=[1,2,3,4,5,6,7,8,9,10,11,12,13,14]
        // let d=[14,12,3,44,35,64,7,8,9,410,611,412,513,514]
        // let d=this.data
        let d=JSON.parse(JSON.stringify(data))
        console.log(d)
        d=this.retun1(d)
        console.log('=======:完成:',d,'次数：',this.count)
      },
       retun1 (arr) {
        if (arr.length === 1) {
          return arr
        }
        let mid=Math.floor((arr.length)/2)
        let leftarr=arr.slice(0,mid)
        let rightarr=arr.slice(mid)
        return this.retun2(this.retun1(leftarr),this.retun1(rightarr))
      },
      retun2(leftarr, rightarr) {
        let r = []
        while (leftarr.length && rightarr.length) {
          if (leftarr[0]<rightarr[0]) {
            r.push(leftarr.shift())
             
          } else {
            r.push(rightarr.shift())
          }
          console.log(this.count)
          console.log(r)
          this.count++
        }
        // console.log('左：',leftarr,'右：',rightarr,'r:',r)
        // console.log('排完：',r.concat(leftarr).concat(rightarr))
        return r.concat(leftarr).concat(rightarr)
      }
    }
  }
</script>
