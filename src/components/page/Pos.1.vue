<template>
    <div class="pos">
        <el-row>
            <el-col :span='7' class="pos-order" id="order-list">
                <el-tabs>
                    <el-tab-pane label="点餐">
                        <el-table :data="tableData" border style="100%">
                            <el-table-column prop="goodsName" label="商品名称"></el-table-column>
                            <el-table-column prop="count" label="量" width="55"></el-table-column>
                            <el-table-column prop="price" label="金额" width="70"></el-table-column>
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
                            <small>金额：</small>{{totalMoney}}元
                        </div>
                        <div class="div-btn">
                            <el-button type="warning">挂单</el-button>
                            <el-button type="danger" @click="delAllGoods">删除</el-button>
                            <el-button type="success" @click="checkout">结账</el-button>
                        </div>
                    </el-tab-pane>
                    <el-tab-pane label="挂单">
                        挂单
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
    },
    methods: {
        // 挂单
        
        // 商品数量不为空，将tableData存本地 ,清空tableData,
        
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
        delSingGoods (goods) {
            this.tableData = this.tableData.filter(o => o.goodsId != goods.goodsId)
            this.getAllMoney()
        },
        // 汇总数量和金钱
        getAllMoney () {
            this.totalCount = 0
            this.totalMoney = 0
            if (this.tableData) {
                this.tableData.forEach((element) => {
                this.totalCount += element.count
                this.totalMoney = this.totalMoney + (element.price * element.count)
            })
            }
        },
        // 删除全部商品
        delAllGoods () {
            this.tableData = []
            this.totalCount = 0
            this.totalMoney = 0
        },
        // 结账
        checkout () {
            if (this.totalCount != 0) {
                this.tableData = []
                this.totalCount = 0
                this.totalMoney = 0
                this.$message({
                    message: '结账成功，感谢你又为店里出了一份力!！',
                    type: 'success'
                })
            }else {
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
</style>
