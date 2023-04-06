<template>
    <ContentBase>
        <div v-for="user in users" :key="user.id" class="card" @click="open_user_profile(user.id)">
            <div class="card-body">
                <div class="row align-items-center">
                    <div class="col-1">
                        <img class="img-fluid" :src="user.photo" alt="">
                    </div>
                    <div class="col-11">
                        <div class="username">{{ user.username }}</div>
                        <div class="followerCount">粉丝：{{ user.followerCount }}</div>
                    </div>
                </div>
            </div>
        </div>
    </ContentBase>
</template>
  
<script>
// @ is an alias to /src
import ContentBase from '@/components/ContentBase.vue';
import { ref } from 'vue';
import $ from 'jquery';
import router from '@/router';
import { useStore } from 'vuex';

export default {
  name: 'UserListView',
  components: {
    ContentBase,
  },
  props: {},
  setup() {
    let users = ref([]);

    $.ajax({
        url: "https://app165.acapp.acwing.com.cn/myspace/userlist/",
        type: "GET",
        success(resp) {
            users.value = resp;
            localStorage.setItem('userlist', users.value);
        },
    });

    const store = useStore();
    const open_user_profile = (userId) => {
        if (store.state.user.is_login) {
            router.push({
                name: "userprofile",
                params: {
                    userId,
                }
            });
        } else {
            router.push({
                name: "login",
            });
        }
    };
    
    return {
      users,
      open_user_profile,
    }
  }
}
</script>

<style scoped>
.card {
  margin-bottom: 10px;
  cursor: pointer;
}

.card:hover {
    box-shadow: 0 3px 4px gray;
    transition: 500ms;
}

img {
  border-radius: 50%;
}

.username {
  font-size: 16px;
  font-weight: bold;
}

.followerCount {
  font-size: 12px;
  font-weight: lighter;
}


</style>
  