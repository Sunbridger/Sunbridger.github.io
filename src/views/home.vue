<style>
    body {
        margin: 0;
        font: Arial,"PingFang SC","Hiragino Sans GB","Microsoft YaHei","WenQuanYi Micro Hei",sans-serif;
    }
    .index-box {
        z-index: 999;
        position: fixed;
        width: 100%;
        background: white;
    }
    .el-tabs__nav-scroll {
        display: flex;
    }
    .el-tabs__nav {
        margin: auto;
    }
    .swiper-container {
        height: calc(100vh - 53px);
        overflow: hidden;
    }
    .swiper-slide {
        overflow-y: scroll;
    }
    .tab-bottom-box {
        padding: 0 12px;
        box-sizing: border-box;
        z-index: 9999;
        position: fixed;
        bottom: 0;
        width: 100%;
        height: 53px;
        background: #fff;
        border-top: 1px solid rgb(114, 107, 107, .6)
    }
    .normal-tab {
        color: rgb(114, 107, 107);
    }
    .is-active-tab {
        color: #409EFF;
    }
</style>

<template>
    <div>
        <swiper :options="swiperOption" ref="mySwiper">
            <swiper-slide>
                <reban />
            </swiper-slide>
            <swiper-slide>
                <taobao :sort="sort" :addItem="addItem"/>
            </swiper-slide>
            <swiper-slide>
                <watchTb />
            </swiper-slide>
            <swiper-slide>
                <shezhi />
            </swiper-slide>
        </swiper>
        <div class="tab-bottom-box flex">
            <p  v-for="item in navTabList"
                :key="item.key"
                @click="handleClick(item.key)"
                :class="[activeIndex === item.key ? 'is-active-tab' : 'normal-tab']">
                {{item.value}}
            </p>
        </div>
    </div>
</template>

<script>

import taobao from './taobao';
import reban from './reban';
import watchTb from './watchTb';
import shezhi from './shezhi';

export default {
    data() {
        return {
            swiperOption: {
                on: {
                    slideChangeTransitionStart: () => {
                        this.activeIndex = this.$refs.mySwiper.swiper.realIndex;
                    }
                }
            },
            activeIndex: 0,
            navTabList: [{
                key: 0,
                value: '微博热搜'
            }, {
                key: 1,
                value: '我的物品'
            }, {
                key: 2,
                value: '收藏商品'
            }, {
                key: 3,
                value: '设置'
            }],
            sort: false,
            addItem: false
        }
    },
    methods: {
        handleClick(i) {
            this.activeIndex = i;
            this.$refs.mySwiper.swiper.slideTo(i);
        }
    },
    watch: {
        activeIndex(i) {
            if (Number(i) === 1) {
                this.sort = localStorage.getItem('sort') === 'true' ? false : true;
                this.addItem = localStorage.getItem('addItem') === 'true' ? true : false;
            } else {
                localStorage.setItem('addItem', false);
            }
        }
    },
    components: {
        taobao,
        reban,
        watchTb,
        shezhi
    }
}
</script>
