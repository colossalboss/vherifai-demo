<template>
    <div class="container">
        <h1 class="text-white">Welcome {{ user.name }} to Cryto Info</h1>

        <div class="row">
            <div class="col-md-12 mb-4" style="color:#788898fa">
                Please kindly verify your details to have access to our full services
            </div>
            <div class="col-md-12">
                <router-link to="/settings" style="width: fit-content" class="btn btn-warning font-weight-bold">Go to settings</router-link >
            </div>
        </div>
    </div>
</template>

<script>
import { ref } from '@vue/reactivity';
import { useRouter } from "vue-router"

export default {
    setup() {
        
        const router = useRouter();

        const user = ref({ })

        const userId = localStorage.getItem('token');
        if (!userId) router.push('/');
        const users = JSON.parse(localStorage.getItem('users'));

          if (!users) {
              router.push('/')
              return false;
          } else {
              
              user.value = users.find(i => i.id === +userId) || { };
              if (!user.value || !user.value.id) {
                  router.push('/')
              }
          }

          return {
              user,
          }
    },
}
</script>
