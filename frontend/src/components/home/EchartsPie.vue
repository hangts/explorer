<template>
  <div class="echarts_component_wrap" :style="`min-width:${minWidth}rem;`">
    <div class="echarts_title_wrap">
      <span class="validators_title"><i class="iconfont iconVotingPower"></i>Validators Top10</span>
      <router-link class="validators_top" :to="`/validators`" @click.native="$uMeng.push('HomeOverview_Validators Top10','click')">
        <span>View All</span>
      </router-link>
    </div>
    <div id="echarts_pie">

    </div>
  </div>


</template>

<script>
    var echarts = require('echarts/lib/echarts')
    require('echarts/lib/component/legend')
    require('echarts/lib/component/tooltip')
    require('echarts/lib/component/title')
    require('echarts/lib/chart/pie');
    require('echarts/lib/component/legendScroll')
  import Tools from "../../util/Tools";

  let pie = null;
  export default {
    name: 'echarts-pie',
    watch:{
      innerWidth(innerWidth){
        //根据设备大小显示饼图的大小
        let radius = innerWidth > 1258 === 1 ? '91%' : '80%';
        let legend = innerWidth > 1258 ?
          {
            orient: 'vertical',
            right: '10%',
            data: [],
            top:30,
          } : {
            orient: 'horizontal',
            bottom:'5%',
            data: [],
            type: 'scroll',
          };
        let center = innerWidth > 1258 ? ['34.2%', '50%'] : ['50%', '45%'];
        let option = {

          tooltip : {
            trigger: 'item',
            formatter(params){
              let res =  `<span style="display:block;color:#00f0ff;padding:0 0.05rem;">${params.name}</span>`;
              if(params.name !== 'others'){
                res += `<span style="display:block;padding:0 0.05rem;">Uptime: ${params.data.upTime}</span>`;
              }
              res += `<span style="display:block;padding:0 0.05rem;">Voting Power: ${(params.value/params.data.totalCount*100).toFixed(2)}%</span>`;
              return res;
            }
          },
          legend,
          series : [
            {
              type: 'pie',
              radius,
              center,
              label:{
                show:false
              },
              data:[],
              itemStyle: {
                emphasis: {
                  shadowBlur: 10,
                  shadowOffsetX: 0,
                  shadowColor: 'rgba(0, 0, 0, 0.5)'
                }
              }
            }
          ]
        };
        if(pie){
          option.legend.data = this.information.legendData;
          option.series[0].data = this.information.seriesData;
          pie.setOption(option);
          pie.on('click',(param)=>{
            if(param.data.name !== 'others'){
              let url = this.addressRoute(param.data.address);
              this.$router.push(url);
            }
          })
        }
      },
      information(information){
        //根据设备大小显示饼图的大小
        let radius = this.innerWidth > 1258 ? '91%' : '80%';
        let legend = this.innerWidth > 1258 ?
          {
            orient: 'vertical',
            right: '5%',
            data: [],
            top:30,
        } : {
          orient: 'horizontal',
            data: [],
            bottom:'5%',
            type: 'scroll',
        };
        let center = this.innerWidth > 1258 ? ['34.2%', '50%'] : ['50%', '45%'];
        let option = {
          tooltip : {
            trigger: 'item',
            formatter(params){
              let res = `<span style="display:block;color:#00f0ff;padding:0 0.05rem;">${params.name}</span>`;
              if(params.name !== 'others'){
                res += `<span style="display:block;padding:0 0.05rem;">Uptime: ${params.data.upTime}</span>`;
              }
              res += `<span style="display:block;padding:0 0.05rem;">Voting Power: ${(params.value/params.data.powerAll*100).toFixed(2)}%</span>`;
              return res;
            }
          },
          legend,
          series : [
            {
              type: 'pie',
              radius,
              center,
              label:{
                show:false
              },
              data:[],
              itemStyle: {
                emphasis: {
                  shadowBlur: 10,
                  shadowOffsetX: 0,
                  shadowColor: 'rgba(0, 0, 0, 0.5)'
                }
              }
            }
          ]
        };
        if(pie){
          option.legend.data = information.legendData;
          option.series[0].data = information.seriesData;
          pie.setOption(option);
          pie.on('click',(param)=>{
            if(param.data.name !== 'others'){
              let url = this.addressRoute(param.data.address);
              this.$router.push(url);
            }

          })
        }
      }
    },
    data() {
      return {
        innerWidth:window.innerWidth,
        minWidth:2.9,

      }
    },
    props:['information'],
    beforeMount() {
      if(this.innerWidth <= 320){
        this.minWidth = 2.9;
      }else if(this.innerWidth <= 375){
        this.minWidth = 3.4;
      }else if(this.innerWidth <= 414){
        this.minWidth = 3.9;
      }
    },
    mounted() {
      pie = echarts.init(document.getElementById('echarts_pie'));
      window.addEventListener('resize',this.onWindowResize)
    },

    methods: {
      onWindowResize(){
        pie.resize();
        this.innerWidth = window.innerWidth;
      },
    },
    beforeDestroy(){
      window.removeEventListener('resize',this.onWindowResize);
    }
  }
</script>
<style lang="scss">
  @import '../../style/mixin';

  .echarts_component_wrap{
    height:3.4rem;
    padding:0.12rem 0.2rem 0 0.2rem;
    background: #fff;
    .echarts_title_wrap{
      height:15%;
      @include flex;
      justify-content: space-between;

      .validators_title{
        font-size:0.18rem;
        i{
          font-size: 0.2rem;
          padding-right: 0.1rem;
          color: #C8D1DA;
        }
      }
      .validators_top{
        span{
          color:var(--bgColor);
          border-bottom: 0.01rem solid var(--bgColor);
          font-size: 0.14rem;
          i{
            display: inline-block;
            padding-left: 0.02rem;
            font-size: 0.14rem;
            transform: rotate(-90deg);
          }
        }
      }
    }
    #echarts_pie{
      width:100%;
      height:85%;
    }
  }



</style>
