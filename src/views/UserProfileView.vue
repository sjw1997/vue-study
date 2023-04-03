<template>
    <ContentBase>
      <div class="row">
        <div class="col-3">
          <UserProfileInfo @follow="follow" @unfollow="unfollow" :user="user" />
          <UserProfileWrite @post_a_post="post_a_post" />
        </div>
        <div class="col-9">
          <UserProfilePosts :posts="posts"/>
        </div>
      </div>
    </ContentBase>
  </template>
  
<script>
// @ is an alias to /src
import ContentBase from '@/components/ContentBase';
import UserProfileInfo from '@/components/UserProfileInfo';
import UserProfilePosts from '@/components/UserProfilePosts';
import UserProfileWrite from '@/components/UserProfileWrite';
import { reactive } from 'vue';

export default {
  name: 'UserProfileView',
  components: {
    ContentBase,
    UserProfileInfo,
    UserProfilePosts,
    UserProfileWrite,
  },
  setup() {
    const user = reactive({
      id: 1,
      username: "sjw",
      lastname: "Shao",
      firstname: "Jiwang",
      followCount: 0,
      is_followed: false,
    });

    const posts = reactive({
      count: 3,
      posts: [
        {
          id: 1,
          userId: 1,
          content: "今天上了Web课，真开心",
        },
        {
          id: 2,
          userId: 1,
          content: "今天上了算法课，真开心",
        },
        {
          id: 3,
          userId: 1,
          content: "今天上了acwing，开心极了",
        },
      ]
    })

    const follow = () => {
      if (user.is_followed) return;
      user.is_followed = true;
      user.followCount ++ ;
    };

    const unfollow = () => {
      if (!user.is_followed) return;
      user.is_followed = false;
      user.followCount -- ;
    };

    const post_a_post = (content) => {
      posts.count ++ ;
      posts.posts.unshift({
        id: posts.count,
        userId: 1,
        content: content,
      });
    };

    return {
      user,
      posts,
      follow,
      unfollow,
      post_a_post,
    };
  }
}
</script>


<style scoped>

</style>