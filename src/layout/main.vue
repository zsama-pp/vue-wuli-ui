<template>
    <div class="gc-main container" :style="styleLayoutMainGroup" :theme="themeActive.name">
        <!-- 顶部导航 -->
        <header-view />
        <!-- 内容 -->
        <div class="gc-body">
            <div class="gc-body__lt sidebar-container" :style="{width: dataBase.h5 ? `${layoutWeb}px` : ''}"  :class="{'gc-h5': dataBase.h5}">
                <sidebarView />
            </div>
            <mInfiniteScroll class="gc-body__rt" :style="{paddingLeft: dataBase.h5 ? `${layoutWeb}px` : ''}" :config="{offset: offsetTop}"> 
                <tagsView />
                <!-- 移除slot插槽方式，采用页面跳转 -->
                <transition name="el-fade-in-linear" mode="out-in"> 
                    <router-view :key="viewKey" />
                </transition>
            </mInfiniteScroll>
        </div>
    </div>
</template>
<script>
/* eslint-disable */
import { mapGetters } from 'vuex'
import { locales } from '@/i18n'
import { headerView, sidebarView, tagsView } from './components'

export default {
    name: 'layout-main',
    data() {
        return {
            navActive: '组件类'
        }
    },
    components: { headerView, sidebarView, tagsView },
    computed: {
        ...mapGetters({
            themeActive: 'theme/activeSetting'
        }),
        ...mapGetters([
            'offsetTop'
        ]),
        layoutWeb() {
            return this.dataBase.h5 ? 75 : 210
        },
        viewKey() {
            return this.$route.fullPath
        },
        /**
         * @description 最外层容器的背景图片样式
         */
        styleLayoutMainGroup() {
            return this.themeActive.backgroundImage
                ? { backgroundImage: `url('${this.themeActive.backgroundImage}')` }
                : {}
        }
    },
    created() {
        // 引入百度地图
        // this.MapInit();
    },
    methods: {
        MapInit(ak = 'c939d01cf713a7826caf8421b9be8c81') {
            var _hmt = _hmt || [];
            (function () {
                var hm = document.createElement("script");
                hm.async = 1;
                hm.src = "http://api.map.baidu.com/api?v=2.0&ak=" + ak + "&callback=init";
                var s = document.getElementsByTagName("script")[0];
                s.parentNode.insertBefore(hm, s);
            })();
        },
        openUrl() {
            // 跳转新链接方式
            let routeUrl = this.$router.resolve({path: '/bi'});
            window.open(routeUrl.href, '_blank');
        }
    },
    locales: locales,
    watch: {
        $route: {
            handler: function(route) {
                this.redirect = route.query && route.query.redirect
            },
            immediate: true
        }
    }
}
</script>

<style lang="less">
/* 框架主体 */
.gc-main {
    position: relative;
    height: 100%;
    width: 100%;
    background-repeat: no-repeat;
    background-position: 50%;
    background-size: cover;
    .gc-body {
        position: relative;
        padding-top: 60px;
        &__lt {
            position: absolute;
            left: 0;
            top: 0;
            padding-top: 60px;
            width: 210px;
            height: 100%;
        }
        &__rt {
            position: relative;
            padding-left: 211px;
            width: 100%;
        }
    }
}
</style>
