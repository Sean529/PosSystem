<template>
    <div class="pos">
        <el-row>
            <el-col :span='7' class="pos-order" id="order-list">
                <el-tabs>
                    <el-tab-pane label="点餐">
                        <el-table :data="tableData" border style="100%" align="left">
                            <el-table-column prop="goodsName" label="商品名称"></el-table-column>
                            <el-table-column prop="count" label="量" width="55"></el-table-column>
                            <el-table-column prop="price" label="单价" width="70"></el-table-column>
                            <el-table-column label="操作" width="100" fixed="right">
                                <template scope="scope">
                                    <el-button type="text" size="small" @click="delSingGoods(scope.row)">删除</el-button>
                                    <el-button type="text" size="small" @click="addOrderList(scope.row)">增加</el-button>
                                </template>
                            </el-table-column>
                        </el-table>
                        <div class="totalDiv">
                            <span class="totalCount">
                                <small>数量：</small>{{totalCount}}</span>
                            <small>总额：</small>{{totalMoney}}元
                        </div>
                        <div class="div-btn">
                            <el-button type="warning" @click="suspendOrder">挂单</el-button>
                            <el-button type="danger" @click="delAllGoods">删除</el-button>
                            <el-button type="success" @click="checkout">结账</el-button>
                        </div>
                    </el-tab-pane>
                    <el-tab-pane label="挂单">
                        <el-table :data="suspendArrOrder" border style="width: 100%" align="left">
                            <el-table-column type="expand">
                                <template scope="props" class="order-details">
                                    <el-table :data="props.row.orderDetails" style="width: 100%" align="left">
                                        <el-table-column prop="goodsName" label="商品名称">
                                        </el-table-column>
                                        <el-table-column prop="count" label="量" width="70">
                                        </el-table-column>
                                        <el-table-column prop="price" label="单价" width="70">
                                        </el-table-column>
                                        <el-table-column label="操作" width="100">
                                            <template scope="scope">
                                                <el-button type="text" size="small" @click="delOrderGoods(props.row, scope.row)">删除</el-button>
                                                <el-button type="text" size="small" @click="addOrderGoods(props.row, scope.row)">增加</el-button>
                                            </template>
                                        </el-table-column>
                                    </el-table>
                                </template>
                            </el-table-column>
                            <el-table-column label="序号">
                                <template scope="scope">
                                    <span>{{scope.row.orderId}}</span>
                                </template>
                            </el-table-column>
                            <el-table-column label="商品">
                                <template scope="scope">
                                    <span>{{scope.row.totalCount}}</span>
                                </template>
                            </el-table-column>
                            <el-table-column label="总额">
                                <template scope="scope">
                                    <span>{{scope.row.totalMoney}}</span>
                                </template>
                            </el-table-column>
                            <el-table-column label="操作" width="100">
                                <template scope="scope">
                                    <el-button type="text" size="small" @click="delSuspendOrder(scope.row)">删除</el-button>
                                    <el-button type="text" size="small" @click="checkoutSuspendOrder(scope.row)">结账</el-button>
                                </template>
                            </el-table-column>
                        </el-table>
                    </el-tab-pane>
                    <el-tab-pane label="外卖">
                        外卖
                    </el-tab-pane>
                </el-tabs>
            </el-col>
            <el-col :span='17'>
                <div class="often-goods">
                    <div class="title">常用商品</div>
                    <div class="often-goods-list">
                        <ul>
                            <li v-for="goods in oftenGoods" @click="addOrderList(goods)" :key="goods.goodsId">
                                <span>{{goods.goodsName}}</span>
                                <span class="o-price">￥{{goods.price}}元</span>
                            </li>
                        </ul>
                    </div>
                    <div class="goods-type">
                        <el-tabs class="goods">
                            <el-tab-pane label="汉堡">
                                <ul class='cookList'>
                                    <li v-for="goods in type0Goods" @click="addOrderList(goods)" :key="goods.goodsId">
                                        <span class="foodImg">
                                            <img :src="goods.goodsImg" width="100%">
                                        </span>
                                        <span class="foodName">{{goods.goodsName}}</span>
                                        <span class="foodPrice">￥{{goods.price}}元</span>
                                    </li>
                                </ul>
                            </el-tab-pane>
                            <el-tab-pane label="小食">
                                <ul class='cookList'>
                                    <li v-for="goods in type1Goods" @click="addOrderList(goods)" :key="goods.goodsId">
                                        <span class="foodImg">
                                            <img :src="goods.goodsImg" width="100%">
                                        </span>
                                        <span class="foodName">{{goods.goodsName}}</span>
                                        <span class="foodPrice">￥{{goods.price}}元</span>
                                    </li>
                                </ul>
                            </el-tab-pane>
                            <el-tab-pane label="薯条">
                                <ul class='cookList'>
                                    <li v-for="goods in type2Goods" @click="addOrderList(goods)" :key="goods.goodsId">
                                        <span class="foodImg">
                                            <img :src="goods.goodsImg" width="100%">
                                        </span>
                                        <span class="foodName">{{goods.goodsName}}</span>
                                        <span class="foodPrice">￥{{goods.price}}元</span>
                                    </li>
                                </ul>
                            </el-tab-pane>
                            <el-tab-pane label="饮料">
                                <ul class='cookList'>
                                    <li v-for="goods in type3Goods" @click="addOrderList(goods)" :key="goods.goodsId">
                                        <span class="foodImg">
                                            <img :src="goods.goodsImg" width="100%">
                                        </span>
                                        <span class="foodName">{{goods.goodsName}}</span>
                                        <span class="foodPrice">￥{{goods.price}}元</span>
                                    </li>
                                </ul>
                            </el-tab-pane>
                        </el-tabs>
                    </div>
                </div>
            </el-col>
        </el-row>
    </div>
</template>
<script>
import axios from 'axios'
export default {
    name: 'Pos',
    data() {
        return {
            tableData: [],
            oftenGoods: [],
            suspendArrOrder: [],
            type0Goods: [],
            type1Goods: [],
            type2Goods: [],
            type3Goods: [],
            totalMoney: 0,
            totalCount: 0
        }
    },
    created() {
        // 读取分类商品列表
        axios.get('http://jspang.com/DemoApi/oftenGoods.php')
            .then(response => {
                // console.log(response)
                this.oftenGoods = response.data
            })
            .catch(error => {
                // console.log(error)
                alert('网络错误，不能访问')
            })
        axios.get('http://jspang.com/DemoApi/typeGoods.php')
            .then(response => {
                // console.log(response)
                this.type0Goods = response.data[0]
                this.type1Goods = response.data[1]
                this.type2Goods = response.data[2]
                this.type3Goods = response.data[3]
            })
    },
    mounted: function () {
        var orderHeight = document.body.clientHeight
        document.getElementById('order-list').style.height = orderHeight + 'px'
        // 初始化挂单 本地同步构造器
        let suspendOrder = JSON.parse(localStorage.getItem('suspendOrder'))
        this.suspendArrOrder = suspendOrder
    },
    methods: {
        // 添加商品
        addOrderList(goods) {
            // 商品是否已经存在于订单列表中
            let isHave = false
            for (let i = 0, len = this.tableData.length; i < len; i++) {
                if (this.tableData[i].goodsId === goods.goodsId) {
                    isHave = true
                }
            }
            // 根据判断的值编写业务逻辑
            if (isHave) {
                // 改变列表中商品数量
                let arr = this.tableData.filter(o => o.goodsId == goods.goodsId)
                arr[0].count++
            } else {
                let newGoods = { goodsId: goods.goodsId, goodsName: goods.goodsName, price: goods.price, count: 1 }
                this.tableData.push(newGoods)
            }
            this.getAllMoney()
        },
        // 删除单个商品
        delSingGoods(goods) {
            this.tableData = this.tableData.filter(o => o.goodsId != goods.goodsId)
            this.getAllMoney()
        },
        // 汇总数量和金钱
        getAllMoney() {
            this.totalCount = 0
            this.totalMoney = 0
            if (this.tableData) {
                this.tableData.forEach((element) => {
                    this.totalCount += element.count
                    this.totalMoney = this.totalMoney + (element.price * element.count)
                })
            }
        },
        // 挂单
        suspendOrder(goods) {
            // 商品数量不为空，清空tableData
            if (this.totalCount != 0) {
                // 订单存本地
                let orderArray = []
                let orderObj = {}
                // 为本组订单生成订单id
                orderObj.totalCount = this.tableData.length //商品种类的数量
                orderObj.totalMoney = this.totalMoney
                orderObj.orderDetails = this.tableData
                orderArray.push(orderObj)
                let suspendOrder = JSON.parse(localStorage.getItem('suspendOrder'))
                if (suspendOrder != null && suspendOrder.length != 0) {
                    // 添加订单ID
                    let orderId = suspendOrder.length
                    orderObj.orderId = orderId
                    // 新的订单 push 到数组中
                    let getOrder = JSON.parse(localStorage.getItem('suspendOrder'))
                    getOrder.push(orderObj)
                    // 数组对象转字符串
                    let suspendOrderStr = JSON.stringify(getOrder)
                    // // 更新本地订单
                    localStorage.setItem('suspendOrder', suspendOrderStr)
                    // 更新构造器
                    this.suspendArrOrder = getOrder
                } else {
                    //添加订单id
                    orderObj.orderId = 0
                    // 将订单信息存本地
                    let orderArrayStr = JSON.stringify(orderArray);
                    localStorage.setItem('suspendOrder', orderArrayStr);
                    // 每次挂单更新数组
                    let getSuspendOrder = JSON.parse(localStorage.getItem('suspendOrder'))
                    this.suspendArrOrder = getSuspendOrder
                }
                // 清空tableData
                this.tableData = []
                this.totalCount = 0
                this.totalMoney = 0
            }
        },
        // 添加商品
        addOrderGoods(order, goods) {
            let arr = this.suspendArrOrder.filter(o => o.orderId == order.orderId)
            let goodsList = arr[0].orderDetails.filter(g => g.goodsId == goods.goodsId)
            goodsList[0].count++
            // 计算金额
            arr[0].totalMoney = arr[0].totalMoney + goods.price
            // 更新本地localStoarge
            localStorage.setItem('suspendOrder', JSON.stringify(this.suspendArrOrder))
        },
        // 挂单 删除某个订单中的商品
        delOrderGoods(order, goods) {
            // 查找本条订单
            let suspendOrder = JSON.parse(localStorage.getItem('suspendOrder'))
            let orderList = suspendOrder.filter(o => o.orderId == order.orderId)
            // 查找本条商品之外的商品
            let goodsList = orderList[0].orderDetails.filter(g => g.goodsId != goods.goodsId)
            // 更新本条订单
            order.orderDetails = goodsList
            order.totalCount = order.orderDetails.length // 更新数量
            order.totalMoney = order.totalMoney - (goods.price * goods.count) //更新金额
            // 数量等于0 删除订单
            if (order.orderDetails.length === 0) {
                // 更新数组 保存本条记录外数据
                let suspendOrder = JSON.parse(localStorage.getItem('suspendOrder'))
                this.suspendArrOrder = suspendOrder.filter(o => o.orderId != order.orderId)
            } else {
                // 订单序号
                let i = order.orderId
                // 更新构造器 
                this.suspendArrOrder[i] = order
            }
            // 更新本地localStoarge
            localStorage.setItem('suspendOrder', JSON.stringify(this.suspendArrOrder))
        },
        // 删除挂单订单
        delSuspendOrder(order) {
            // 更新数组 保存本条记录外数据
            let suspendOrder = JSON.parse(localStorage.getItem('suspendOrder'))
            let so = suspendOrder.filter(o => o.orderId != order.orderId)
            // 更新orderId
            var arr = []
            for (let index = 0; index < so.length; index++) {
                let element = so[index];
                element.orderId = index
                arr.push(element)
            }
            this.suspendArrOrder = arr
            // 更新本地
            localStorage.setItem('suspendOrder', JSON.stringify(this.suspendArrOrder))
        },
        // 挂单结账
        checkoutSuspendOrder(order) {
            // 清空本条记录
            // 更新数组 保存本条记录外数据
            let suspendOrder = JSON.parse(localStorage.getItem('suspendOrder'))
            this.suspendArrOrder = suspendOrder.filter(o => o.orderId != order.orderId)
            // 更新本地
            localStorage.setItem('suspendOrder', JSON.stringify(this.suspendArrOrder))
            this.$message({
                message: '结账成功，感谢你又为店里出了一份力!！',
                type: 'success'
            })
        },
        // 删除全部商品
        delAllGoods() {
            this.tableData = []
            this.totalCount = 0
            this.totalMoney = 0
        },
        // 结账
        checkout() {
            if (this.totalCount != 0) {
                this.tableData = []
                this.totalCount = 0
                this.totalMoney = 0
                this.$message({
                    message: '结账成功，感谢你又为店里出了一份力!！',
                    type: 'success'
                })
            } else {
                this.$message.error('不能空结。老板理解你急切的心情')
            }
        }
    }
}
</script>
<style scoped>
.pos-order {
    background-color: #F9FAFC;
    border-right: 1px solid #C0CCDA;
}

.div-btn {
    margin-top: 10px;
}

.title {
    height: 20px;
    border-bottom: 1px solid #D3DCE6;
    background-color: #F9FAFC;
    padding: 10px;
    text-align: left;
}

.often-goods-list ul li {
    list-style: none;
    float: left;
    border: 1px solid #E5E9F2;
    padding: 10px;
    margin: 10px;
    background-color: #fff;
    cursor: pointer;
}

.o-price {
    color: #58B7FF;
}

.goods-type {
    clear: both;
}

.cookList li {
    list-style: none;
    width: 23%;
    border: 1px solid #E5E8F2;
    overflow: hidden;
    background-color: #fff;
    padding: 2px;
    float: left;
    margin: 2px;
    cursor: pointer;
}

.cookList li span {
    display: block;
    float: left;
}

.foodImg {
    width: 40%;
}

.foodName {
    font-size: 14px;
    padding-left: 10px;
    color: brown;
}

.foodPrice {
    font-size: 16px;
    padding-left: 10px;
    padding-top: 10px;
}

.totalDiv {
    background-color: #fff;
    padding: 10px;
    border-bottom: 1px solid #d3dce6;
}

.totalCount {
    margin-right: 20px;
}

.order-details {
    padding: 0;
}
</style>
