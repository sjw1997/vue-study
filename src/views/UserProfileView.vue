<template>
    <ContentBase>
        <div class="row">
            <div class="col-3">
                <UserProfileInfo :user="user" @change_follow_status="change_follow_status"/>
                <UserProfileWrite v-if="is_me" @post_a_post="post_a_post"/>
            </div>
            <div class="col-9">
                <UserProfilePosts :user="user" :posts="posts" @delete_a_post="delete_a_post"/>
            </div>
        </div>
    </ContentBase>
</template>

<script>
// @ is an alias to /src
import ContentBase from '@/components/ContentBase.vue';
import { reactive, ref } from 'vue';
import UserProfilePosts from '@/components/UserProfilePosts.vue'
import UserProfileInfo from '@/components/UserProfileInfo.vue';
import UserProfileWrite from '@/components/UserProfileWrite.vue';
import { useRoute } from 'vue-router';
import $ from 'jquery';
import { useStore } from 'vuex';
import { computed } from '@vue/reactivity';

export default {
    name: 'UserProfileView',
    components: {
        ContentBase,
        UserProfilePosts,
        UserProfileInfo,
        UserProfileWrite,

    },
    setup() {
        const posts = reactive({});
        const user = reactive({});
        let is_me = ref();

        const route = useRoute();
        const user_id = parseInt(route.params.userId);
        const store = useStore();

        is_me = computed(() => user_id === store.state.user.id);

        $.ajax({
            url: "https://app165.acapp.acwing.com.cn/myspace/getinfo/",
            type: "GET",
            data: {
                user_id,
            },
            headers: {
                "Authorization": `Bearer ${store.state.user.access}`,
            },
            success(resp) {
                user.id = resp.id;
                user.username = resp.username;
                user.followerCount = resp.followerCount;
                user.is_followed = resp.is_followed;
                user.photo = resp.photo;
            }
        });

        $.ajax({
            url: "https://app165.acapp.acwing.com.cn/myspace/post/",
            type: "GET",
            headers: {
                "Authorization": `Bearer ${store.state.user.access}`
            },
            data: {
                user_id,
            },
            success(resp) {
                posts.posts = resp;
                posts.count = resp.length;
            }
        });

        const change_follow_status = () => {
            user.is_followed = !user.is_followed;
            if (user.is_followed) {
                user.followerCount ++ ;
            } else {
                user.followerCount -- ;
            }
        };

        const post_a_post = (content) => {
            posts.count ++ ;
            posts.posts = [
                {
                    id: posts.count,
                    content,
                },
                ...posts.posts
            ]
        };

        const delete_a_post = (target_post_id) => {
            posts.posts = posts.posts.filter(post => post.id !== target_post_id);
        }

        return {
            posts,
            user,
            change_follow_status,
            is_me,
            post_a_post,
            delete_a_post,
        }
    }
}
</script>

<style scoped>

</style>
  