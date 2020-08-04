<template>
    <div>
        <h3>中国疫情</h3>
        <p class="txt">来源：各地官方通报及权威媒体报道</p>
        <p class="txt">更新：2020-07-16 23:51:00</p>

        <div class="flexbox">
            <div class="item">
                <div class="red bold">2357</div>
                <div>现存确诊</div>
                <div class="little">
                    <span class="gray">昨日</span>
                    <span class="red">40</span>
                </div>
            </div>
            <div class="item">
                <div class="orange bold">88</div>
                <div>现存疑似</div>
            </div>
            <div class="item"></div>
        </div>
        <div class="flexbox">
            <div class="item">
                <div class="red bold">2357</div>
                <div>现存确诊</div>
                <div class="little">
                    <span class="gray">昨日</span>
                    <span class="red">40</span>
                </div>
            </div>
            <div class="item">
                <div class="orange bold">88</div>
                <div>现存疑似</div>
            </div>
            <div class="item"></div>
        </div>
        
        <!--echarts图表-->
        <div id="bar" style="width:100%; height: 400px;"></div>

        <div id="map" style="width:100%; height: 400px;"></div>

        <!--表格-->
        <div>
            <div class="row">
                <div class="column">地区</div>
                <div class="column">确诊</div>
                <div class="column">死亡</div>
                <div class="column">治愈</div>
            </div>

            <div v-for="item in showlist">
                <div class="row bg-gray" @click="toggleCity(item)" >
                    <div class="column">{{item.provinceShortName}}</div>
                    <div class="column">{{item.confirmedCount}}</div>
                    <div class="column">{{item.deadCount}}</div>
                    <div class="column">{{item.curedCount}}
                        <font-awesome-icon :icon="['fas','caret-down']" v-if="!item.expanded" />
                        <font-awesome-icon :icon="['fas','caret-up']" v-if="item.expanded" />
                    </div>
                </div>
                <div class="cities" v-if="item.expanded">
                    <div class="row gray" v-for="city in item.cities">
                        <div class="column">{{city.cityName}}</div>
                        <div class="column">{{city.confirmedCount}}</div>
                        <div class="column">{{city.deadCount}}</div>
                        <div class="column">{{city.curedCount}}</div>
                    </div>
                </div>
            </div>
            

            <!-- <div class="row bg-gray" @click="toggleCity">
                <div class="column">湖北</div>
                <div class="column">67803</div>
                <div class="column">3212</div>
                <div class="column">64014
                    <font-awesome-icon :icon="['fas','caret-down']" v-if="!expanded" />
                    <font-awesome-icon :icon="['fas','caret-up']" v-if="expanded" />
                </div>
            </div>
            <div class="cities">
                <div class="row gray">
                    <div class="column">武汉1</div>
                    <div class="column">50008</div>
                    <div class="column">2571</div>
                    <div class="column">46863</div>
                </div>
            </div> -->

            

        </div>

        <div id="msg" @click="toggleData">
            {{ msg }}
        </div>

    </div>
</template>

<script>

import echarts from 'echarts'
import '../../node_modules/echarts/map/js/china.js'

export default {
    name: "Main",
    data () {
        return {
            msg: "查看更多地区",
            flag: true,
            newslist:[],
            showlist:[]
        }
    },
    created () {
        this.$axios.get('http://api.tianapi.com/txapi/ncovcity/index?key=99010f95ded4b1cfc47129c42f8904f9')
        .then(res=>{
            console.log(res);
            this.newslist = res.data.newslist;
            this.showlist = this.newslist.slice(0,6);
            var maplist = [];
            for(var i = 0; i < this.newslist.length; i++)
            {
                var obj = {};
                obj.name = this.newslist[i].provinceShortName;
                obj.value = this.newslist[i].confirmedCount;

                maplist.push(obj);
            }
            var myChart = echarts.init(document.getElementById("map"));

            var option = {
                tooltip: {
                    trigger: 'item'
                },
                //左侧小导航图标
                visualMap: {  
                    show : true,  
                    x: 'left',  
                    y: 'bottom',
                    textStyle:{
                        
                    },
                    splitList: [
                        {start: 1000},{start: 500, end: 999},{start: 100, end: 499},
                        {start: 10, end: 99},{start: 1, end: 9},
                    ],  
                    color: ['#cc0000', '#ff4d4d', '#ff9999','#ffe5e5']
                },  
                //配置属性
                series:[{
                    name: '中国疫情地图',
                    type: 'map',
                    mapType: 'china',
                    label: {
                        normal: {
                            show: true, //默认显示省份名称
                            fontSize: 8
                        },
                        emphasis: {
                            show: true
                        }
                    },
                    data:maplist
                }]
            };

            myChart.setOption(option);

        })
        .catch(res=>{

        })
    },
    mounted(){

        


        var myChart2 = echarts.init(document.getElementById('bar'));

        // 指定图表的配置项和数据
        var posList = [
            'left', 'right', 'top', 'bottom',
            'inside',
            'insideTop', 'insideLeft', 'insideRight', 'insideBottom',
            'insideTopLeft', 'insideTopRight', 'insideBottomLeft', 'insideBottomRight'
        ];

        var app = {};

        app.configParameters = {
            rotate: {
                min: -90,
                max: 90
            },
            align: {
                options: {
                    left: 'left',
                    center: 'center',
                    right: 'right'
                }
            },
            verticalAlign: {
                options: {
                    top: 'top',
                    middle: 'middle',
                    bottom: 'bottom'
                }
            },
            position: {
                options: echarts.util.reduce(posList, function (map, pos) {
                    map[pos] = pos;
                    return map;
                }, {})
            },
            distance: {
                min: 0,
                max: 100
            }
        };

        app.config = {
            rotate: 90,
            align: 'left',
            verticalAlign: 'middle',
            position: 'insideBottom',
            distance: 15,
            onChange: function () {
                var labelOption = {
                    normal: {
                        rotate: app.config.rotate,
                        align: app.config.align,
                        verticalAlign: app.config.verticalAlign,
                        position: app.config.position,
                        distance: app.config.distance
                    }
                };
                myChart.setOption({
                    series: [{
                        label: labelOption
                    }, {
                        label: labelOption
                    }, {
                        label: labelOption
                    }, {
                        label: labelOption
                    }]
                });
            }
        };


        var labelOption = {
            show: true,
            position: app.config.position,
            distance: app.config.distance,
            align: app.config.align,
            verticalAlign: app.config.verticalAlign,
            rotate: app.config.rotate,
            formatter: '{c}  {name|{a}}',
            fontSize: 16,
            rich: {
                name: {
                    textBorderColor: '#fff'
                }
            }
        };

        var optionBar = {
            color: ['#003366', '#006699', '#4cabce', '#e5323e'],
            tooltip: {
                trigger: 'axis',
                axisPointer: {
                    type: 'shadow'
                }
            },
            legend: {
                data: ['Forest', 'Steppe', 'Desert', 'Wetland']
            },
            toolbox: {
                show: true,
                orient: 'vertical',
                left: 'right',
                top: 'center',
                feature: {
                    mark: {show: true},
                    dataView: {show: true, readOnly: false},
                    magicType: {show: true, type: ['line', 'bar', 'stack', 'tiled']},
                    restore: {show: true},
                    saveAsImage: {show: true}
                }
            },
            xAxis: [
                {
                    type: 'category',
                    axisTick: {show: false},
                    data: ['2012', '2013', '2014', '2015', '2016']
                }
            ],
            yAxis: [
                {
                    type: 'value'
                }
            ],
            series: [
                {
                    name: 'Forest',
                    type: 'bar',
                    barGap: 0,
                    label: labelOption,
                    data: [320, 332, 301, 334, 390]
                },
                {
                    name: 'Steppe',
                    type: 'bar',
                    label: labelOption,
                    data: [220, 182, 191, 234, 290]
                },
                {
                    name: 'Desert',
                    type: 'bar',
                    label: labelOption,
                    data: [150, 232, 201, 154, 190]
                },
                {
                    name: 'Wetland',
                    type: 'bar',
                    label: labelOption,
                    data: [98, 77, 101, 99, 40]
                }
            ]
        };
        // 使用刚指定的配置项和数据显示图表。
        myChart2.setOption(optionBar);

    },
    methods:{
        toggleCity(item) {
            console.log(item);
            // var column = item.target;
            // var row = column.parentElement; //原生js
            // //row = item.path[1]; // 获得当前 element的爸爸，path是一个array，存当前element的祖宗
            // //console.log(row);
            // var cities = row.nextElementSibling;
            //console.log(cities);
            //设置元素的 display 属性
            // if (cities.style.display == "block") {
            //     cities.style.display = "none";
            // }
            // else {
            //     cities.style.display = "block";
            // }
            // item.expanded = !item.expanded;
            this.$set(item, "expanded", !item.expanded)
        },
        toggleData() {
            if (this.flag) {
                this.msg = "收起更多地区";
                this.showlist = this.newslist;
            }
            else {
                this.msg = "查看更多地区";
                this.showlist = this.newslist.slice(0, 6);
            }
            this.flag = !this.flag;
            console.log(this.msg);
        }
    }
}
</script>

<style scoped>
    /* .cities 
    {
        display: none;
    } */
    .container 
    {
        height: 455px;
        overflow: hidden;
    }
    #msg 
    {
        height: 60px;
        line-height: 60px;
        text-align: center;
    }
    .bg-gray
    {
        background-color: #ececec;
    }
    .row 
    {
        height: 60px;
        display: flex;
        margin-bottom: 5px;
        border-radius: 20px;
    }
    .column
    {
        width: 25%;
        text-align: center;
        line-height: 60px;  /**垂直居中，让行高和div高度一致 */
    }
    .little 
    {
        font-size: 14px;
    }
    .orange 
    {
        color: orange;
    }
    .gray 
    {
        color: gray;
    }
    .red 
    {
        color: rgb(196, 28, 28);
    }
    .bold 
    {
        font-size: 18px;
        font-weight: bold;
    }
    .txt 
    {
        font-size: 14px;
        color: gray;
    }
    .flexbox 
    {
        width: 100%;
        height: 90px;
        background-color: lightgray;
        border-radius: 5px;
        display: flex;
        justify-content: center; /**主轴（一般情况是横轴）方向 */
        align-items: center;  /**纵轴方向 */
        margin-bottom: 10px;
    }
    .item 
    {
        width: 80px;
        height: 80px;
        border: 1px solid red;
        margin: 0 50px  ;
        display: flex;
        flex-direction: column;
        justify-content: center;
        align-items: center;
    }

</style>