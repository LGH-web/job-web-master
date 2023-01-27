<template>
    <swiper class="swiper" :options="swiperOption">
    <swiper-slide>
        <img   src="../assets/images/1.png" width="800px" height="360px">
    </swiper-slide>
    <swiper-slide>
        <img  src="../assets/images/2.png" width="800px" height="360px">
    </swiper-slide>
    <swiper-slide>
        <img   src="../assets/images/3.png" width="800px" height="360px">
    </swiper-slide>
        <swiper-slide>
            <img   src="../assets/images/4.png" width="800px" height="360px">
        </swiper-slide>
        <swiper-slide>
            <img   src="../assets/images/5.png" width="800px" height="360px">
        </swiper-slide>
        <swiper-slide>
            <img   src="../assets/images/6.png" width="800px" height="360px">
        </swiper-slide>
        <swiper-slide>
            <img   src="../assets/images/7.png" width="800px" height="360px">
        </swiper-slide>
        <!--分页器-->
    <div class="swiper-pagination" slot="pagination"></div>
        <div class="swiper-button-prev"></div>
        <div class="swiper-button-next"></div>


    </swiper>
</template>

<script>
    // vue-awesome-swiper
    import {Swiper, SwiperSlide} from 'vue-awesome-swiper';
    import 'swiper/css/swiper.css'

    export default {
        name: "RSwiper",
        components: {Swiper, SwiperSlide},
        data() {
            return {
                swiperOption: {
                    pagination: {
                        el: '.swiper-pagination',
                        clickable :true, // 分页导航是否可点击
                    },
                    loop: true, // 环路(无缝滚动)
                    speed: 600, // 切换速度

                        autoplay: {
                        delay: 5000,
                        // 用户操作不停止自动播放
                        disableOnInteraction: false,

                    } //5000，即5秒切换一次

                }
            }
        }
    }
</script>

<style scoped>

</style>
