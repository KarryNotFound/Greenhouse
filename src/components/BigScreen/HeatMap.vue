<template>
    <div>
        <h1>大棚热力数据图</h1>
        <!-- {{option.xAxis.data}} -->
        <!-- {{obj.perm}} -->
        <!-- {{option.series[0].data}} -->
    <div class="myHeatMap" ref="heatmap"></div>
    </div>
</template>

<script>
import * as echarts from "echarts";
import "lib-flexible/flexible.js";
export default {
    data(){
        return{
        myChart:null,
        noise:null,
        obj:{
        xData:[],
        yData:[],
        data1:[],
        perm:[],
        gradP:[],
        p:[],
        grad3:[],
        },
        option:{
            tooltip: {
    },
    grid: {
      right: 120,
      left: 30,
      top: 40,
    },
    xAxis: {
      type: 'category',
      data: [],
      axisLabel:{
        color:'white',
      },
    },
    yAxis: {
      type: 'category',
      data: [],
      axisLabel:{
        color:'white',
      },
    },
    
    visualMap: {
      type: 'piecewise',
      textStyle:{
        color:'white',
      },
      min: 0,
      max: 1,
      left: 'right',
      top:'30%',
      calculable: true,
      realtime: false,
      splitNumber: 8,
      inRange: {
        color: [
          '#313695',
          '#4575b4',
          '#74add1',
          '#abd9e9',
          '#e0f3f8',
          '#ffffbf',
          '#fee090',
          '#fdae61',
          '#f46d43',
          '#d73027',
          '#a50026'
        ]
      }
    },
    series: [
      {
        name: 'Gaussian',
        type: 'heatmap',
        data: [],
        emphasis: {
          itemStyle: {
            borderColor: 'white',
            borderWidth: 1
          }
        },
        progressive: 1000,
        animation: false
      }
    ]
        }
        }
  },

    mounted(){
      
        this.drawEcharts();
  
        this.initChart();
      
    },
    methods:{
        initChart(_this){    
            this.getNoiseHelper();
            this.seed(Math.random());
            this.generateData(2, -5, 5);
            this.myChart.setOption(this.option);

            window.addEventListener("resize",this.resizefun);
        },
        drawEcharts(){
        
            this.myChart = echarts.init(this.$refs.heatmap);
         
            this.myChart.setOption(this.option);

            window.addEventListener("resize",this.resizefun);
        },
        resizefun(){
          this.myChart.resize();
        },
        generateData(theta, min, max) {
            var data1 = [];
            var xData = [];
            var yData = [];
                for(let j = 0 ; j <=200 ; j++){
            yData.push(j);

    }
        for (let i = 0; i <= 200; i++) {
                for (let j = 0; j <= 200; j++) {
                  data1.push([i, j,this.perlin2(i/40,j/20)+0.5]);
             }
             xData.push(i);
    }
      this.$set(this.option.xAxis,"data",xData);
      this.$set(this.option.yAxis,"data",yData);
      this.$set(this.option.series[0],"data",data1);
   
  },
       getNoiseHelper() {
        class Grad {
        constructor(x, y, z) {
        this.x = x;
        this.y = y;
        this.z = z;
      }
      dot2(x, y) {
        return this.x * x + this.y * y;
      }
      dot3(x, y, z) {
        return this.x * x + this.y * y + this.z * z;
      }
    }
    let grad3 = [
      new Grad(1, 1, 0),
      new Grad(-1, 1, 0),
      new Grad(1, -1, 0),
      new Grad(-1, -1, 0),
      new Grad(1, 0, 1),
      new Grad(-1, 0, 1),
      new Grad(1, 0, -1),
      new Grad(-1, 0, -1),
      new Grad(0, 1, 1),
      new Grad(0, -1, 1),
      new Grad(0, 1, -1),
      new Grad(0, -1, -1)
    ];
    this.$set(this.obj,"grad3",grad3);

    let p = [
      151, 160, 137, 91, 90, 15, 131, 13, 201, 95, 96, 53, 194, 233, 7, 225, 140,
      36, 103, 30, 69, 142, 8, 99, 37, 240, 21, 10, 23, 190, 6, 148, 247, 120,
      234, 75, 0, 26, 197, 62, 94, 252, 219, 203, 117, 35, 11, 32, 57, 177, 33,
      88, 237, 149, 56, 87, 174, 20, 125, 136, 171, 168, 68, 175, 74, 165, 71,
      134, 139, 48, 27, 166, 77, 146, 158, 231, 83, 111, 229, 122, 60, 211, 133,
      230, 220, 105, 92, 41, 55, 46, 245, 40, 244, 102, 143, 54, 65, 25, 63, 161,
      1, 216, 80, 73, 209, 76, 132, 187, 208, 89, 18, 169, 200, 196, 135, 130,
      116, 188, 159, 86, 164, 100, 109, 198, 173, 186, 3, 64, 52, 217, 226, 250,
      124, 123, 5, 202, 38, 147, 118, 126, 255, 82, 85, 212, 207, 206, 59, 227,
      47, 16, 58, 17, 182, 189, 28, 42, 223, 183, 170, 213, 119, 248, 152, 2, 44,
      154, 163, 70, 221, 153, 101, 155, 167, 43, 172, 9, 129, 22, 39, 253, 19, 98,
      108, 110, 79, 113, 224, 232, 178, 185, 112, 104, 218, 246, 97, 228, 251, 34,
      242, 193, 238, 210, 144, 12, 191, 179, 162, 241, 81, 51, 145, 235, 249, 14,
      239, 107, 49, 192, 214, 31, 181, 199, 106, 157, 184, 84, 204, 176, 115, 121,
      50, 45, 127, 4, 150, 254, 138, 236, 205, 93, 222, 114, 67, 29, 24, 72, 243,
      141, 128, 195, 78, 66, 215, 61, 156, 180
    ];

    this.$set(this.obj,"p",p);

    // To remove the need for index wrapping, double the permutation table length
    let perm = new Array(512);
    let gradP = new Array(512);
    this.$set(this.obj,"perm",perm);
    this.$set(this.obj,"gradP",gradP);
    // This isn't a very good seeding function, but it works ok. It supports 2^16
    // different seed values. Write something better if you need more seeds.
    this.seed(0);  
},
     seed(seed) {
      if (seed > 0 && seed < 1) {
        // Scale the seed out
        seed *= 65536;
      }
      seed = Math.floor(seed);
      if (seed < 256) {
        seed |= seed << 8;
      }
      for (let i = 0; i < 256; i++) {
        let v;
        if (i & 1) {
          v = this.obj.p[i] ^ (seed & 255);
        } else {
          v = this.obj.p[i] ^ ((seed >> 8) & 255);
        }
        // this.perm[i] = this.perm[i+256] = v;
        this.$set(this.obj.perm,i,v);
        this.$set(this.obj.perm,i+256,v);

        // this.gradP[i] = this.gradP[i + 256] = this.grad3[v % 12];
        this.$set(this.obj.gradP,i,this.obj.grad3[v%12]);
        this.$set(this.obj.gradP,i+256,this.obj.grad3[v%12]);
      }
    },
    fade(t) {
      return t * t * t * (t * (t * 6 - 15) + 10);
    },
    lerp(a, b, t) {
      return (1 - t) * a + t * b;
    },
    // 2D Perlin Noise
    perlin2(x, y) {
      // Find unit grid cell containing point
      let X = Math.floor(x),
        Y = Math.floor(y);
      // Get relative xy coordinates of point within that cell
      x = x - X;
      y = y - Y;
      // Wrap the integer cells at 255 (smaller integer period can be introduced here)
      X = X & 255;
      Y = Y & 255;
      // Calculate noise contributions from each of the four corners
      let n00 = this.obj.gradP[X + this.obj.perm[Y]].dot2(x,y);
      let n01 = this.obj.gradP[X + this.obj.perm[Y + 1]].dot2(x, y - 1);
      let n10 = this.obj.gradP[X + 1 + this.obj.perm[Y]].dot2(x - 1, y);
      let n11 = this.obj.gradP[X + 1 + this.obj.perm[Y + 1]].dot2(x - 1, y - 1);
      // Compute the fade curve value for x
      let u = this.fade(x);
      // Interpolate the four results
      return this.lerp(this.lerp(n00, n10, u), this.lerp(n01, n11, u), this.fade(y));
    },
    }
}

</script>

<style scoped>
h1{
    height:10px;
    color:white;
    line-height:30px;
    text-align:center;
    font-size:15px;
}

.myHeatMap{
position:relative;
height:350px;
top:15px;
bottom:0;

}
</style>