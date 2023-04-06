<template>
    <div class="card">
        <div class="card-body">
            <div v-for="post in posts.posts" :key="post.id" class="card" style="margin-bottom: 15px;">
                <div class="card-body">
                    <div class="post_time">
                        <span>发表于</span>
                        <span>XXXX-XX-XX</span>
                    </div>
                    <div>{{ post.content }}</div>
                    <button v-if="is_me" @click="delete_a_post(post.id)" type="button" class="btn btn-danger btn-sm">删除此帖</button>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
import { computed } from '@vue/reactivity';
import $ from 'jquery';
import { ref } from 'vue';
import { useStore } from 'vuex';

export default {
    name: "UserProfilePosts",
    props: {
        posts: {
            type: Object,
            required: true,
        },
        user: {
            type: Object,
            required: true,
        },
    },
    setup(props, context) {
        let is_me = ref();
        const store = useStore();


        is_me = computed(() => store.state.user.id === props.user.id);

        const delete_a_post = (target_post_id) => {
            $.ajax({
                url: "https://app165.acapp.acwing.com.cn/myspace/post/",
                type: "DELETE",
                data: {
                    post_id: target_post_id,
                },
                headers: {
                    "Authorization": `Bearer ${store.state.user.access}`
                },
                success(resp) {
                    if (resp.result === "success") {
                        context.emit("delete_a_post", target_post_id);
                    }
                },
            });
        };

        return {
            delete_a_post,
            is_me,
        }
    }
}

</script>

<style scoped>
.post_time {
    font-size: 12px;
    color: gray;
    margin-bottom: 5px;
}

span {
    margin-right: 5px;
}

button {
    float: right;
}
</style>