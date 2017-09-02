<template>
    <div>
        <nav-header></nav-header>
        <nav-bread>
            <span slot="bread">Goods</span>
        </nav-bread>
        <div class="accessory-result-page accessory-page">
            <div class="container">
                <div class="filter-nav">
                    <span class="sortby">Sort by:</span>
                    <a href="javascript:void(0)" class="default cur">Default</a>
                    <a href="javascript:void(0)" class="price">Price
                        <svg class="icon icon-arrow-short">
                            <use xlink:href="#icon-arrow-short"></use>
                        </svg>
                    </a>
                    <a href="javascript:void(0)" class="filterby stopPop" @click="showFilterPop">Filter by</a>
                </div>
                <div class="accessory-result">
                    <!-- filter -->
                    <div class="filter stopPop" id="filter" v-bind:class="{'filterby-show':filterBy}">
                        <dl class="filter-price">
                            <dt>Price:</dt>
                            <dd @click="setPriceFilter('all')">
                                <a href="javascript:void(0)" v-bind:class="{'cur':priceChecked=='all'}">All</a>
                            </dd>
                            <dd v-for="(price,index) in priceFilterList" @click="setPriceFilter(index)">
                                <a href="javascript:void(0)" v-bind:class="{'cur':priceChecked==index}">{{price.startPrice}} - {{price.endPrice}}</a>
                            </dd>
                        </dl>
                    </div>

                    <!-- search result accessories list -->
                    <div class="accessory-list-wrap">
                        <div class="accessory-list col-4">
                            <ul>
                                <li v-for="(item,index) in goodsList">
                                    <div class="pic">
                                        <a href="#"><img v-lazy="'/static/img/'+item.ProductPic" alt=""></a>
                                    </div>
                                    <div class="main">
                                        <div class="name">{{item.ProductName}}</div>
                                        <div class="price">{{item.ProductPrice}}</div>
                                        <div class="btn-area">
                                            <a href="javascript:;" class="btn btn--m">加入购物车</a>
                                        </div>
                                    </div>
                                </li>
                            </ul>
                        </div>
                    </div>
                </div>
            </div>
        </div>
        <div class="md-overlay" v-show="overlayFlag" @click="closePop"></div>
        <nav-footer></nav-footer>
    </div>
</template>

<script>
import './../assets/css/base.css'
import './../assets/css/product.css'
import NavHeader from '@/components/navheader'
import NavFooter from '@/components/navfooter'
import NavBread from '@/components/navbread'
import axios from 'axios'
export default {
    data() {
        return {
            goodsList: [],
            priceFilterList:[
                {
                    "startPrice":0.00,
                    "endPrice":500.00
                },
                {
                    "startPrice":500.00,
                    "endPrice":1000.00
                },
                {
                    "startPrice":1000.00,
                    "endPrice":2000.00
                }
            ],
            priceChecked:'all',
            filterBy:false,
            overlayFlag:false
        }
    },
    components:{
        NavHeader,
        NavFooter,
        NavBread
    },
    mounted:function(){
        this.getGoodsList();
    },
    methods:{
        getGoodsList(){
            axios.get('/goods').then(result=>{
                var res = result.data;
                this.goodsList = res.Result;
            })
        },
        showFilterPop(){
            this.filterBy=true;
            this.overlayFlag=true;
        },
        setPriceFilter(index){
            this.priceChecked=index;
            this.closePop();
        },
        closePop(){
            this.filterBy=false;
            this.overlayFlag=false;
        }
    }
}
</script>
