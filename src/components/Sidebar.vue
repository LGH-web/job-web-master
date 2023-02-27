<template>
    <el-aside width="200px">
        <div class="logo">
            <img src="../assets/images/logo.png">
            <div style="margin-left: 40px">高校就业服务平台</div>
        </div>
        <el-menu
                background-color="#20262E"
                text-color="#ffffff"
                active-text-color="#1AA095"
                style="border: 0"
                :default-active="active"
                router
        >
            <el-menu-item @click="handleMenuItem(item)" :index="item.href" v-for="(item,index) in menus" :key="index">
                <i :class="item.icon"></i>
                <span slot="title">{{item.name}}</span>
            </el-menu-item>
        </el-menu>
    </el-aside>
</template>

<script>
    export default {
        name: "Sidebar",
        data() {
            return {
                menus: [],
                active: ''
            }
        },
        created() {
            this.$store.dispatch('getMenus').then(res => {
                if (res) {
                    this.menus = res.data;
                    let selectedMenu;
                    if (this.$route.path) {
                        this.active = this.$route.name;
                        selectedMenu = this.menus.filter(v => v.href == this.active)[0];
                    } else {
                        this.active = this.menus[0].href;
                        selectedMenu = this.menus[0];
                    }
                    this.handleMenuItem(selectedMenu);
                }
            })
        },
        methods: {
            handleMenuItem(item) {
                this.$emit('handleMenuItem', item)
            }
        }
    }
</script>

<style scoped>
    .el-aside {
        background-color: #20262E;
    }

    .logo {
        height: 60px;
        width: 200px;
        color: #ffffff;
        font-size: 18px;
        font-weight: bold;
        text-align: center;
        line-height: 60px;
    }

    .logo img {
        position: absolute;
        width: 40px;
        height: 40px;
        left: 3px;
        top: 8px;
    }

    .el-menu-item {
        border-bottom: 1px solid #444444;
    }
</style>
