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
        
        <!--表格-->
        <div :class="[flag ? classA : '']">
            <div class="row">
                <div class="column">地区</div>
                <div class="column">确诊</div>
                <div class="column">死亡</div>
                <div class="column">治愈</div>
            </div>

            <div class="row bg-gray">
                <div class="column">香港</div>
                <div class="column">914</div>
                <div class="column">4</div>
                <div class="column">216
                    <font-awesome-icon :icon="['fas','caret-down']" />
                </div>
            </div>

            <div class="row bg-gray" @click="toggleCity">
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
                <div class="row gray">
                    <div class="column">武汉2</div>
                    <div class="column">50008</div>
                    <div class="column">2571</div>
                    <div class="column">46863</div>
                </div>
                <div class="row gray">
                    <div class="column">武汉3</div>
                    <div class="column">50008</div>
                    <div class="column">2571</div>
                    <div class="column">46863</div>
                </div>
            </div>

            <div class="row bg-gray" @click="toggleCity">
                <div class="column">上海</div>
                <div class="column">67803</div>
                <div class="column">3212</div>
                <div class="column">64014</div>
            </div>
            <div class="cities">
                <div class="row gray">
                    <div class="column">上海1</div>
                    <div class="column">50008</div>
                    <div class="column">2571</div>
                    <div class="column">46863</div>
                </div>
                <div class="row gray">
                    <div class="column">上海2</div>
                    <div class="column">50008</div>
                    <div class="column">2571</div>
                    <div class="column">46863</div>
                </div>
                <div class="row gray">
                    <div class="column">上海3</div>
                    <div class="column">50008</div>
                    <div class="column">2571</div>
                    <div class="column">46863</div>
                </div>
            </div>

            <div class="row bg-gray">
                <div class="column">湖北</div>
                <div class="column">67803</div>
                <div class="column">3212</div>
                <div class="column">64014</div>
            </div>

            <div class="row bg-gray">
                <div class="column">湖北</div>
                <div class="column">67803</div>
                <div class="column">3212</div>
                <div class="column">64014</div>
            </div>

            <div class="row bg-gray">
                <div class="column">广东</div>
                <div class="column">67803</div>
                <div class="column">3212</div>
                <div class="column">64014</div>
            </div>

            <div class="row bg-gray">
                <div class="column">湖北</div>
                <div class="column">67803</div>
                <div class="column">3212</div>
                <div class="column">64014</div>
            </div>

            <div class="row bg-gray">
                <div class="column">湖北</div>
                <div class="column">67803</div>
                <div class="column">3212</div>
                <div class="column">64014</div>
            </div>

            <div class="row bg-gray">
                <div class="column">湖北</div>
                <div class="column">67803</div>
                <div class="column">3212</div>
                <div class="column">64014</div>
            </div>

            <div class="row bg-gray">
                <div class="column">湖北</div>
                <div class="column">67803</div>
                <div class="column">3212</div>
                <div class="column">64014</div>
            </div>

            <div class="row bg-gray">
                <div class="column">湖北</div>
                <div class="column">67803</div>
                <div class="column">3212</div>
                <div class="column">64014</div>
            </div>

            <div class="row bg-gray">
                <div class="column">湖北2</div>
                <div class="column">67803</div>
                <div class="column">3212</div>
                <div class="column">64014</div>
            </div>

        </div>

        <div id="msg" @click="toggleData">
            {{ msg }}
        </div>

    </div>
</template>

<script>
export default {
    name: "Main",
    data () {
        return {
            msg: "查看更多地区",
            flag: true,
            classA: 'container',
            expanded: false
        }
    },
    methods:{
        toggleCity(e) {
            console.log(e);
            var column = e.target;
            var row = column.parentElement; //原生js
            //row = e.path[1]; // 获得当前 element的爸爸，path是一个array，存当前element的祖宗
            console.log(row);
            var cities = row.nextElementSibling;
            console.log(cities);
            //设置元素的 display 属性
            if (cities.style.display == "block") {
                cities.style.display = "none";
            }
            else {
                cities.style.display = "block";
            }
            this.expanded = !this.expanded;
            
        },
        toggleData() {
            if (this.flag) {
                this.msg = "收起更多地区";
            }
            else {
                this.msg = "查看更多地区";
            }
            this.flag = !this.flag;
            console.log(this.msg);
        }
    }
}
</script>

<style scoped>
    .cities 
    {
        display: none;
    }
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