<template>
<div class="flight-item">
    <div @click="clishow">
        <!-- 显示的机票信息 -->
        <el-row type="flex" align="middle" class="flight-info">
            <el-col :span="6">
                <span>{{data.airline_name}} </span> {{data.flight_no}}
            </el-col>
            <el-col :span="12">
                <el-row type="flex" justify="space-between" class="flight-info-center">
                    <el-col :span="8" class="flight-airport">
                        <strong>{{data.dep_time}}</strong>
                        <span>{{data.org_airport_name}}{{data.org_airport_quay}}</span>
                    </el-col>
                    <el-col :span="8" class="flight-time">
                        <span>{{surtime}}</span>
                    </el-col>
                    <el-col :span="8" class="flight-airport">
                        <strong>{{data.arr_time}}</strong>
                        <span>{{data.dst_airport_name}}{{data.dst_airport_quay}}</span>
                    </el-col>
                </el-row>
            </el-col>
            <el-col :span="6" class="flight-info-right">
                ￥<span class="sell-price">{{data.base_price/2}}</span>起
            </el-col>
        </el-row>
    </div>
    <div class="flight-recommend" v-if="isshow">
        <!-- 隐藏的座位信息列表 -->
        <el-row type="flex" v-for="(item,index) in data.seat_infos" :key="index" justify="space-between" align="middle">
            <el-col :span="4">低价推荐</el-col>
            <el-col :span="20">
                <el-row type="flex" justify="space-between" align="middle" class="flight-sell">
                    <el-col :span="16" class="flight-sell-left">
                        <span>{{item.name}}</span> | {{item.supplierName}}
                    </el-col>
                    <el-col :span="5" class="price">
                        ￥{{item.settle_price}}
                    </el-col>
                    <el-col :span="3" class="choose-button">
                        <el-button type="warning" size="mini" @click="xuanding(data.id,item.seat_xid)">
                            选定
                        </el-button>
                        <p>剩余：{{item.discount}}</p>
                    </el-col>
                </el-row>
            </el-col>
        </el-row>
    </div>
</div>
</template>

<script>
export default {
    data() {
        return {
            isshow: false
        }
    },
    methods: {
        // 点击显示或隐藏
        clishow() {
            this.isshow = !this.isshow
        },
        // 点击选定跳转
        xuanding(id, seat_xid) {
            this.$router.push({
                path: '/air/order',
                query: {
                    id,
                    seat_xid
                }
            })
        }
    },
    computed: {
        surtime() {
            const dep = this.data.dep_time.split(':') //开始
            const arr = this.data.arr_time.split(':') //结束
            const depVal = dep[0] * 60 + +dep[1] //开始时间转为分钟
            let arrVal = arr[0] * 60 + +arr[1] //结束时间转为分钟
            // 判断结束时间如果小于开始时间则说明是第二天了
            if (arrVal < depVal) {
                //所以在结束时间中加上一天的分钟数
                arrVal += 24 * 60
            }
            return `${Math.floor((arrVal-depVal)/60)}时${(arrVal-depVal)%60}分`
        }
    },
    props: {
        // 数据
        data: {
            type: Object,
            // 默认是空数组
            default: {}
        }
    }
}
</script>

<style lang="less" scoped>
.flight-item {
    border: 1px #ddd solid;
    margin-bottom: 10px;

    .flight-info {
        padding: 15px;
        cursor: pointer;

        >div {

            &:first-child,
            &:last-child {
                text-align: center;
            }
        }
    }

    .flight-info-center {
        padding: 0 30px;
        text-align: center;

        .flight-airport {
            strong {
                display: block;
                font-size: 24px;
                font-weight: normal;
            }

            span {
                font-size: 12px;
                color: #999;
            }
        }

        .flight-time {
            span {
                display: inline-block;
                padding: 10px 0;
                border-bottom: 1px #eee solid;
                color: #999;
            }
        }
    }

    .flight-info-right {

        .sell-price {
            font-size: 24px;
            color: orange;
            margin: 0 2px;
        }
    }
}

.flight-recommend {
    background: #f6f6f6;
    border-top: 1px #eee solid;
    padding: 0 20px;

    .flight-sell {
        border-bottom: 1px #eee solid;
        padding: 10px 0;

        &:last-child {
            border-bottom: none;
        }

        .flight-sell-left {
            font-size: 12px;

            span {
                color: green;
            }
        }

        .price {
            font-size: 20px;
            color: orange;
        }

        .choose-button {
            text-align: center;
            color: #666;

            button {
                display: block;
                width: 100%;
                margin-bottom: 5px;
            }
        }
    }
}
</style>
