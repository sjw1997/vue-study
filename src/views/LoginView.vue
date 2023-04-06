<template>
    <ContentBase>
        <div class="login">
            <form @submit.prevent="login">
                <div class="mb-3">
                    <label for="username" class="form-label">用户名</label>
                    <input v-model="username" type="text" class="form-control" id="username">
                </div>
                <div class="mb-3">
                    <label for="password" class="form-label">密码</label>
                    <input v-model="password" type="password" class="form-control" id="password">
                </div>
                <div class="error_message">{{ error_message }}</div>
                <button type="submit" class="btn btn-primary">登录</button>
            </form>
        </div>
    </ContentBase>
</template>
  
<script>
  // @ is an alias to /src
import ContentBase from '@/components/ContentBase.vue';
import router from '@/router';
import { ref } from 'vue';
import { useStore } from 'vuex';

export default {
name: 'LoginView',
    components: {
        ContentBase,
    },
    props: {},
    setup() {
        let error_message = ref('');
        let username = ref('');
        let password = ref('');

        const store = useStore();

        const login = () => {
            error_message.value = "";
            store.dispatch("login", {
                username: username.value,
                password: password.value,
                success() {
                    router.push({name: 'userlist'});
                },
                error() {
                    error_message.value = "用户名或密码错误";
                }
            });
        };

        return {
            error_message,
            login,
            username,
            password,
        }
    }
}
</script>

<style scoped>
.login {
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
  