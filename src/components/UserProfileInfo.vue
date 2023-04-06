<template>
    <div class="card">
        <div class="card-body">
            <div class="row">
                <div class="col-4" style="margin-bottom: 10px;">
                    <img class="img-fluid" :src="user.photo">
                </div>
                <div class="col-8">
                    <div class="username">{{ user.username }}</div>
                    <div class="follwerCount">粉丝：{{ user.followerCount }}</div>
                    <button @click="change_follow_status(user.id)" v-if="!is_me && !user.is_followed" type="button" class="btn btn-primary btn-sm">+关注</button>
                    <button @click="change_follow_status(user.id)" v-if="!is_me && user.is_followed" type="button" class="btn btn-secondary btn-sm">取消关注</button>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
import { computed } from '@vue/reactivity'; 
import { useStore } from 'vuex';
import $ from 'jquery';

export default {
    name: "UserProfileInfo",
    props: {
        user: {
            type: Object,
            required: true,
        }
    },
    setup(props, context) {
        const store = useStore();

        let is_me = computed(() => props.user.id === store.state.user.id);

        const change_follow_status = (target_id) => {
            $.ajax({
                url: "https://app165.acapp.acwing.com.cn/myspace/follow/",
                type: "POST",
                data: {
                    target_id,
                },
                headers: {
                    "Authorization": `Bearer ${store.state.user.access}`
                },
                success(resp) {
                    if (resp.result === "success") {
                        context.emit("change_follow_status");
                    }
                }
            });
        };

        return {
            is_me,
            change_follow_status,
        }
    }
}
</script>

<style scoped>
img {
    border-radius: 50%;
}
.username {
    font-size: 19px;
    font-weight: bold;
    margin-bottom: 5px;
}

.follwerCount {
    font-size: 12px;
    color: gray;
    margin-bottom: 5px;
}

</style>