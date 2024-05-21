<script setup>
import CategoryProducts from "~/components/common/CategoryProducts.vue";
import { ref } from 'vue'
import {ShoppingCart} from "@element-plus/icons-vue";
import {fakeProducts} from "@/constants/fakeData.js"
import spaFetch from "~/plugins/fetch.js";


const { $apiUrl } = useNuxtApp()

const products = ref(fakeProducts)
const router = useRouter()
const value = ref('Option1')
const loading = ref(false)

const formState = reactive({
    page: 1,
    size: 20,
    total: 0
})

const getProducts = () =>{
    loading.value = true
    spaFetch(false)($apiUrl.PRODUCT,{
        method: 'GET',
        params:{
            page: formState.page,
            pageSize: formState.size,
            search: 'smartphone',
        },

    }).then( res => {
        products.value = res.results.map(item =>{
            return {
                ...item,
                status: item.type
            }
        })
        loading.value = false
        formState.total = res.count
    }).catch( error => {
        loading.value = false
        console.log("error", error.response)
    })
}

const advertising =
    {
        img1:'data:image/jpeg;base64,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',
        img2:'data:image/jpeg;base64,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',
    }


const options = [
    {
        value: 'Option1',
        label: 'Mặc định',
    },
    {
        value: 'Option2',
        label: 'Tên A-Z',
    },
    {
        value: 'Option3',
        label: 'Tên Z-A',
    },
    {
        value: 'Option4',
        label: 'Giá giảm dần',
    },
    {
        value: 'Option5',
        label: 'Giá tăng dần',
    },
]

const handleClick = (item) => {
    router.push(item && item.id ? `/product/${item.id}` : '')
}

const handleChangePage = (value) => {
    products.value = []
    getProducts()
}

getProducts()
</script>

<template>
    <div>
       <div v-loading="loading" class="w-4/5 flex flex-col items-center justify-center  mx-auto">
           <CategoryProducts>
               <img :src="advertising && advertising.img1 ? advertising.img1 : ''"
                    alt="">
               <div class="flex items-center justify-center p-3 bg-red-600 my-4 text-white font-semibold">
                    HELLO SHOP - CHUYÊN PHÂN PHỐI SMARTPHONE CHÍNH HÃNG, XÁCH TAY
               </div>
               <img :src="advertising && advertising.img2 ? advertising.img2 : ''" alt="">
<!--                <div class="flex h-16 items-center bg-gray-500 px-8">-->
<!--                    <div>-->
<!--                        <span class="text-white">-->
<!--                        Sắp xếp theo:-->
<!--                        </span>-->
<!--                        <el-select-->
<!--                            v-model="value"-->
<!--                            class="m-2"-->
<!--                            default-first-option-->
<!--                            size="small"-->
<!--                            style="width: 120px;"-->
<!--                        >-->
<!--                            <el-option-->
<!--                                v-for="item in options"-->
<!--                                :key="item.value"-->
<!--                                :label="item.label"-->
<!--                                :value="item.value"-->
<!--                            />-->
<!--                        </el-select>-->
<!--                    </div>-->
<!--                </div>-->
               <div class="w-full grid grid-cols-4 gap-4">
                   <div v-for="(product, index) in products"
                                 :key="index"
                                 class="mt-4 hover:mt-1 card-new-product">
                       <div class="w-full bg-white border border-solid border-gray-300 rounded-xl overflow-hidden cursor-pointer" @click="handleClick(product)">
                           <div class="w-full relative">
                               <img class="w-full h-full" :src="product.img" alt="">
                               <div class="w-full py-2 text-center text-white bg-red-500 uppercase font-semibold absolute bottom-0">
                                   {{ product.status }}
                               </div>
                           </div>
                           <div class=" h-[5rem] text-sm p-3 text-center font-semibold hover:text-blue-800 ">
                               {{ product.name }}
                           </div>
                           <div class="p-2 text-center border-solid border-t-gray-200 border border-b-blue-300 border-b-2 text-red-600 font-semibold">
                               {{ product.price }}đ
                           </div>
                           <div class="py-2 hover text-center font-bold text-blue-600 uppercase flex justify-center items-center gap-1 hover:text-white hover:bg-red-600 transition duration-200">
                               <el-icon size="20"> <ShoppingCart/> </el-icon>
                               Thêm vào giỏ
                           </div>
                       </div>
                   </div>
               </div>
               <el-pagination
                   v-model:current-page="formState.page"
                   :page-size="formState.size"
                   layout="prev, pager, next"
                   :total="formState.total"
                   @currentChange="handleChangePage"
               />
           </CategoryProducts>
       </div>
    </div>
</template>

<style lang="scss">

</style>