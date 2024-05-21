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
    rePassword:'',
})

const validatePass = (rule, value, callback) => {
    if (value === '') {
        callback(new Error('Vui lòng nhập mật khẩu!'))
    } else {
        if (formLabelAlign.rePassword !== '') {
            if (!form.value) return
            form.value.validateField('rePassword', () => null)
        }
        callback()
    }
}
const validatePass2 = (rule, value, callback) => {
    if (value === '') {
        callback(new Error('Vui lòng nhập mật khẩu xác nhận!'))
    } else if (value !== formLabelAlign.password) {
        callback(new Error("Mật khẩu xác nhận không khớp!"))
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
    username: [
        {
            trigger: 'blur',
            validator: validateUsername,
        },
    ],
    password: [{
        trigger: 'blur',
        validator: validatePass,
    }],
    rePassword: [{
        trigger: 'blur',
        validator: validatePass2,
    }],
});



const handleRegister = async () => {
    await form.value.validate(async (valid, fields) => {
        if (valid) {
            loading.value = true
            const myHeaders = new Headers();
            myHeaders.append("Content-Type", "application/json");
            const raw = JSON.stringify({
                "username": formLabelAlign.username,
                "password": formLabelAlign.password
            });
            fetch(config.public.baseURL+$apiUrl.REGISTER,{
                method: 'POST',
                headers: myHeaders,
                body: raw,
                redirect: "follow",
            }).then( async res => {
                loading.value = false
                const result = await res.json()
                if (res.ok) {
                    ElMessage.success(
                        "Đã đăng ký thành công"
                    )
                    await router.push('/login')
                }
                else {
                    console.log("error", result)
                    if (result && result.username && result.username[0]) {
                        ElMessage.error(
                            "Tài khoản đã tồn tại"
                        )
                    }else {
                        ElMessage.error(
                            "Vui lòng kiểm tra lại thông tin"
                        )
                    }
                }
            })
        }
    });
};

auth.deleteAuth()
</script>

<template>
    <div class="w-full flex items-center justify-center min-h-screen bg-gray-100">
        <div class="w-full max-w-md p-8 space-y-6 rounded-xl bg-white shadow-md">
            <h1 class="text-2xl font-bold text-center uppercase">Đăng ký</h1>
            <el-form
                ref="form"
                :rules="rules"
                label-position="top"
                :model="formLabelAlign"
                style="max-width: 460px"
            >
                <el-form-item prop="username" label="Tài khoản">
                    <el-input v-model="formLabelAlign.username"
                              @keypress.enter="handleRegister" />
                </el-form-item>
                <el-form-item prop="password" label="Mật khẩu">
                    <el-input v-model="formLabelAlign.password"
                              type="password"
                              show-password
                              @keypress.enter="handleRegister"/>
                </el-form-item>
                <el-form-item  prop="rePassword" label="Nhập lại mật khẩu">
                    <el-input v-model="formLabelAlign.rePassword"
                              type="password"
                              show-password
                              @keypress.enter="handleRegister"/>
                </el-form-item>
                <el-button
                    type="primary"
                    :loading="loading"
                    class="w-full py-2 text-white rounded-md"
                    @click="handleRegister">
                    Đăng ký
                </el-button>
            </el-form>
        </div>
    </div>
</template>

<style lang="scss">

</style>