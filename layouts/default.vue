<script setup>
import MenuItem from "@/components/common/MenuItem.vue"
import { Search, ShoppingCart } from "@element-plus/icons-vue"
import {useAuthStore} from "~/stores/auth.js";
import spaFetch from "~/plugins/fetch.js";


const route = useRoute()
const router = useRouter()
const auth = useAuthStore()
const {$apiUrl} = useNuxtApp()

const search = ref("")
const menu = ref([
    {
        isSubmenu: false,
        title: 'Trang chủ',
        link: '/'
    },
    {
        isSubmenu: true,
        title: 'Smart phone',
        link: '/category/smartphone/',
        children: [
            {img: 'https://herogame.vn/upload/images//full/185202319930_259036466157a22b624.17302350_pngwing.com%20(2).png', title: 'Mô hình Pokemon', link: '/category/pokemon/'},
            {img: 'https://herogame.vn/upload/images//full/1852023192517_609666466192db7af76.30878542_1280px-Pok%C3%A9mon_Trading_Card_Game_logo.svg.png', title: 'Thẻ bài TCG', link: '/category/pokemontcg/'},
            {img: 'https://herogame.vn/upload/images//full/1852023192946_7277464661a3ac40c38.45394227_pngegg.png', title: 'Mô hình OnePiece', link: '/category/onepiece/'},
            {img: 'https://herogame.vn/upload/images//full/1852023193347_7807964661b2be0c445.24402127_pngegg%20(1).png', title: 'Mô hình DragonBall', link: '/category/dragonball/'},
            {img: 'https://herogame.vn/upload/images//full/1852023193627_5817664661bcbeafb71.52683629_pngwing.com%20(3).png', title: 'Mô hình Doraemon', link: '/category/doraemon/'},
            {img: 'https://herogame.vn/upload/images//full/185202319460_5670164661e08c6a3c7.81656300_pngwing.com%20(5).png', title: 'Mô hình khác', link: '/category/other/'},
        ],
        mainSubMenu: {img: 'https://herogame.vn/upload/images//full/1852023185051_199356466111b558247.94879207_rx_93_v2_hi_nu_gundam_by_zephyrnic-d5hewkq.png', title: 'Mô hình Gundam', link: '#'},
        colorBg: "bg-amber-600",
        colorItemSM: "bg-yellow-500"
    },
    {
        isSubmenu: true,
        title: 'Laptop',
        link: '/category/laptop/',
        children: [
            {img: 'https://i.imgur.com/pBgpztm.jpeg', title: 'Handmade', link: '/category/t1Handmade/'},
            {img: 'https://i.ebayimg.com/images/g/3K8AAOSwBNFlXBbY/s-l1600.png', title: 'Mô hình T1', link: '/category/t1Figure/'},
            {img: 'https://i.imgur.com/CG1Phhk.png', title: 'Balo T1', link: '/category/t1Backpack/'},
        ],
        mainSubMenu: {img: 'https://i.imgur.com/zJvS12E.png', title: 'Đồng phục T1', link: '/category/t1Clothes/'},
        colorBg: "bg-red-500",
        colorItemSM: "bg-black"
    },
    {
         title: 'PC',
        link: '/category/pc/',
        children: [
            {img: 'https://i.imgur.com/pBgpztm.jpeg', title: 'Handmade', link: '/category/t1Handmade/'},
            {img: 'https://i.ebayimg.com/images/g/3K8AAOSwBNFlXBbY/s-l1600.png', title: 'Mô hình T1', link: '/category/t1Figure/'},
            {img: 'https://i.imgur.com/CG1Phhk.png', title: 'Balo T1', link: '/category/t1Backpack/'},
        ],
        mainSubMenu: {img: 'https://i.imgur.com/zJvS12E.png', title: 'Đồng phục T1', link: '/category/t1Clothes/'},
        colorBg: "bg-red-500",
        colorItemSM: "bg-black"
    },
    {
        isSubmenu: false,
        title: 'Gọi mua hàng',
        link: '#',
    },
])

onMounted(() => {
    let prevScrollpos = window.pageYOffset;
    window.onscroll = function() {
        const currentScrollPos = window.pageYOffset;
        if (prevScrollpos > currentScrollPos && currentScrollPos > 120) {
            document.getElementById("navbar").style.top = "-140px";
        } else if (currentScrollPos <= 120) {
            document.getElementById("navbar").style.top = "0px"
        } else {
            document.getElementById("navbar").style.top = "-200px"
        }
        prevScrollpos = currentScrollPos;
    }
})
const handleClick = () => {
    router.push("/")
    console.log(route)
}

const logOut = () => {
    auth.deleteAuth()
    window.location.reload()
}

const getCart = () =>{
    const id = auth.$state.user && auth.$state.user.cart ? auth.$state.user.cart : null
    spaFetch()(`${$apiUrl.CART}${id}/`,{
        method: 'GET'
    }).then( res => {
        auth.setQuantityInCart(res.products.length)
    }).catch( error => {
        console.log("error", error.response)
    })
}

getCart()

watch(search, () => {
    router.push('/category/search/')
    auth.setSearch(search.value)
})
</script>

<template>
    <div class="h-screen relative">
        <div id="navbar">
            <div class="h-[40px] flex items-center justify-end p-2 bg-red-500 text-white">
               <div v-if="!auth.$state.accessToken || !auth.$state.refreshToken">
                   <nuxt-link to="/login" class="cursor-pointer hover:underline hover:text-red-700">
                       Đăng nhập
                   </nuxt-link>
                   <el-divider direction="vertical"/>
                   <nuxt-link to="/register" class="cursor-pointer hover:underline hover:text-red-700">
                       Đăng ký
                   </nuxt-link>
               </div>
                <div v-else>
                    <span>
                        Xin chào, {{auth.$state.user.username}}
                    </span>
                    <el-divider direction="vertical"/>
                    <nuxt-link to="/myorder">
                       <span class="cursor-pointer hover:underline hover:text-red-700">
                            Đơn hàng của tôi
                       </span>
                    </nuxt-link>
                    <el-divider direction="vertical"/>
                    <span class="cursor-pointer hover:underline hover:text-red-700" @click="logOut">
                        Đăng xuất
                    </span>
                </div>
            </div>
            <div class="w-full h-[100px] flex flex-row justify-center items-center space-x-32">
                <div    @click="handleClick"
                        class="font-bold cursor-pointer" >
                    HELLO SHOP
                </div>
                <div class="w-1/3 border-solid border-4">
                    <el-input
                        v-model="search"
                        placeholder="Nhập từ khoá cần tìm"
                        class="input-with-select"
                        size="large"
                        :suffix-icon="Search"
                    >
                    </el-input>
                </div>
                <el-badge :value="auth.$state.quantityInCart" class="cursor-pointer hover:text-blue-600 transition duration-200" @click="$router.push('/cart')">
                    <el-icon size="40"><ShoppingCart/></el-icon>
                </el-badge>
            </div>
            <div class="flex justify-center text-xl font-bold relative bg-black">
                <MenuItem v-for="(item, index) in menu"
                          :key="index"
                          :item="item"
                          :class="item.isSubmenu ? 'menu-item-wrap' : ''"/>
            </div>
        </div>

        <div class=" mt-[200px]">
            <slot/>
        </div>
    </div>
</template>

<style lang="scss" >
.el-message{
    z-index: 9999 !important;
}
</style>