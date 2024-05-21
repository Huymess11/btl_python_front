<script setup>
import TableProduct from "~/components/manage/TableProduct.vue";
import {useAuthStore} from "~/stores/auth.js";

definePageMeta({
    layout: "admin",
    middleware: "auth"
})

import TableOrder from "~/components/manage/TableOrder.vue";
import TableUser from "~/components/manage/TableUser.vue";

const active = ref('1')
const auth = useAuthStore()

const logOut = () => {
    auth.deleteAuth()
    window.location.reload()
}

const handleChange = (key) => {
    console.log(key)
    active.value = key
}
</script>

<template>
    <div class="w-full h-screen">
        <div class="w-full h-full flex">
            <el-menu
                active-text-color="#ffd04b"
                background-color="#DC143C"
                default-active="1"
                text-color="#fff"
                class="w-60 h-full"
                @select="handleChange"
            >
                <el-menu-item index="1">
                    <span>Quản lý sản phẩm</span>
                </el-menu-item>
                <el-menu-item index="2">
                    <span>Quản lý đơn hàng</span>
                </el-menu-item>
                <el-menu-item index="3">
                    <span>Quản lý tài khoản</span>
                </el-menu-item>
                <div class="flex w-full items-center justify-center mt-16">
                    <el-button type="primary" plain @click="logOut">Log Out</el-button>
                </div>
            </el-menu>
            <TableProduct v-if="active === '1'" class="flex-1 overflow-auto"/>
            <TableOrder v-if="active === '2'" class="flex-1 overflow-auto"/>
            <TableUser v-if="active === '3'" class="flex-1 overflow-auto"/>
        </div>
    </div>
</template>

<style lang="scss">

</style>