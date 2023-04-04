<template>
    <div class="card">
        <div class="card-body">
            <div v-for="post in posts.posts" :key="post.id" class="card single-post">
                <div class="card-body">
                    {{ post.content }}
                    <button v-if="is_me" 
                    type="button" 
                    class="btn btn-danger btn-sm"
                    @click="delete_a_post(post.id)">
                        删除
                    </button>
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
    name: "UserProfilePosts",
    props: {
        posts: {
            type: Object,
            required: true,
        },
        user: {
            type: Object,
            required: true,
        }
    },
    setup(props, context) {
        const store = useStore();

        let is_me = computed(() => (props.user.id === store.state.user.id));

        const delete_a_post = post_id => {
            $.ajax({
                url: "https://app165.acapp.acwing.com.cn/myspace/post/",
                type: "DELETE",
                data: {
                    post_id,
                },
                headers: {
                    'Authorization': "Bearer " + store.state.user.access,
                },
                dataType: "json",
                success(resp) {
                    if (resp.result === 'success') {
                        context.emit("delete_a_post", post_id);
                    }
                }
            });
        };

        return {
            is_me,
            delete_a_post,
        }
    }
}
</script>

<style scoped>
.single-post {
    margin-bottom: 10px;
}

button {
    float: right;
}
</style>