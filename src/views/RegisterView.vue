<template>
    <ContentBase>
        <div class="register">
            <form>
                <div class="mb-3">
                    <label for="username" class="form-label">用户名</label>
                    <input v-model="username" type="text" class="form-control" id="username">
                </div>
                <div class="mb-3">
                    <label for="password" class="form-label">密码</label>
                    <input v-model="password" type="password" class="form-control" id="password">
                </div>
                <div class="mb-3">
                    <label for="password_confirm" class="form-label">确认密码</label>
                    <input v-model="password_confirm" type="password" class="form-control" id="password_confirm">
                </div>
                <div class="error_message">{{ error_message }}</div>
                <button @click="register" type="button" class="btn btn-primary">注册</button>
            </form>
        </div>
    </ContentBase>
</template>

<script>
// @ is an alias to /src
import ContentBase from '@/components/ContentBase.vue';
import { ref } from 'vue';
import $ from 'jquery';
import { useStore } from 'vuex';
import router from '@/router';

export default {
    name: 'RegisterView',
    components: {
        ContentBase,
    },
    setup() {
        let error_message = ref('');
        let username = ref('');
        let password = ref('');
        let password_confirm = ref('');

        const store = useStore();
        const register = () => {
            error_message.value = "";
            $.ajax({
                url: "https://app165.acapp.acwing.com.cn/myspace/user/",
                type: "POST",
                data: {
                    username: username.value,
                    password: password.value,
                    password_confirm: password_confirm.value,
                },
                success(resp) {
                    if (resp.result === "success") {
                        store.dispatch("login", {
                            username: username.value,
                            password: password.value,
                            success() {
                                router.push({name: 'userlist'});
                            },
                            error() {
                                error_message.value = "系统异常，请稍后再试";
                            }
                        });
                    } else {
                        error_message.value = resp.result;
                    }
                }
            });
        };

        return {
            error_message,
            username,
            password,
            password_confirm,
            register,
        }
    }
}
</script>

<style scoped>
.register {
width: 40%;
margin: 0 auto;
}

button {
width: 100%;
}
.error_message {
    color: red;
    margin-bottom: 5px;
}
</style>
  