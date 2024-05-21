<script setup >
definePageMeta({
    layout: "custom"
})
import {useAuthStore} from "~/stores/auth.js";


const auth = useAuthStore()
const { $apiUrl } = useNuxtApp()
const config = useRuntimeConfig();
const router = useRouter()

const form = ref();
const loading = ref(false);

const formLabelAlign = reactive({
    username: '',
    password: '',
})

const validatePass = (rule, value, callback) => {
    if (value === '') {
        callback(new Error('Vui lòng nhập mật khẩu!'))
    } else {
        callback()
    }
}

const validateUsername = (rule, value, callback) => {
    if (value === '') {
        callback(new Error('Vui lòng nhập tài khoản!'))
    } else {
        callback()
    }
}

const rules = reactive({
    username: [{
            trigger: 'blur',
            validator: validateUsername,
        }],
    password: [{
        trigger: 'blur',
        validator: validatePass,
    }],
});


const handleLogin = async () => {
    await form.value.validate(async (valid, fields) => {
        if (valid) {
            loading.value = true
            const myHeaders = new Headers();
            myHeaders.append("Content-Type", "application/json");
            const raw = JSON.stringify({
                "username": formLabelAlign.username,
                "password": formLabelAlign.password
            });
            fetch(config.public.baseURL+$apiUrl.LOGIN,{
                method: 'POST',
                headers: myHeaders,
                body: raw,
                redirect: "follow",
            }).then( async res => {
                loading.value = false
                const result = await res.json()
                if (res.ok) {
                    auth.setAccessToken(result.access)
                    auth.setRefreshToken(result.refresh)
                    auth.setUser(result.user)
                    if (result.user.is_superuser) await router.push('/manage')
                    else await router.push('/')
                }
                else {
                    console.log("error", result)
                    ElMessage.error(
                        "Tài khoản hoặc mật khẩu không chính xác!"
                    )
                }
            })
        }
    });
};

auth.deleteAuth()
</script>

<template>
    <div class="flex items-center justify-center min-h-screen bg-gray-100 ">
        <div class="w-full max-w-md p-8 space-y-6 rounded-xl bg-white shadow-md">
            <h1 class="text-2xl font-bold text-center uppercase">Đăng nhập</h1>
            <el-form
                ref="form"
                :rules="rules"
                label-position="top"
                :model="formLabelAlign"
            >
                <el-form-item prop="username"
                              label="Tài khoản">
                    <el-input v-model="formLabelAlign.username"
                              @keypress.enter="handleLogin"/>
                </el-form-item>
                <el-form-item prop="password" label="Mật khẩu">
                    <el-input v-model="formLabelAlign.password"
                              type="password"
                              @keypress.enter="handleLogin"
                              show-password/>
                </el-form-item>
                <el-form-item>
                    <div class="w-full flex items-center justify-between">
                        <div class="flex items-center gap-x-1 text-sm">
                            Chưa có tài khoản? <nuxt-link to="/register" class="cursor-pointer text-blue-500 hover:underline"> Đăng ký </nuxt-link>
                        </div>
                        <a href="#" class="text-sm text-blue-500 hover:underline"> Quên mật khẩu? </a>
                    </div>
                </el-form-item>
                <el-button
                    type="primary"
                    class="w-full py-2 text-white rounded-md"
                    @click="handleLogin"
                    :loading="loading">
                    Đăng nhập
                </el-button>
            </el-form>
        </div>
    </div>
</template>

<style lang="scss">

</style>