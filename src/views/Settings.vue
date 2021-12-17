<template>
    <div class="container">
        <div class="row">
            <div class="col-md-12">
                <h2 class="text-white">Account Settings</h2>
            </div>
            <div class="col-md-12">
                <hr style="border: 1px solid #788898fa">
            </div>

            <div class="col-md-12" style="color:#788898fa">
                <p>
                    <span>Full name: </span>
                    <span>{{ user.name }} </span>
                </p>
                <p>
                    <span>Email </span>
                    <span>{{ user.email }}</span>
                </p>
                <hr style="border: 1px solid #788898fa">
                
            </div>

            <div class="col-md-12 mt-4">
                <h4 class="text-white">Level 1 verification (Single verification)</h4>
            </div>
            <div class="col-md-12" style="color:#788898fa">
                <p class="">
                    <span class="mr-3">Verify your BVN </span>
                    <span class="text-danger font-weight-bold" v-if="!user.verificationStatus1">Undone</span>
                    <span class="text-success font-weight-bold" v-else>Done</span>
                </p>
                <p style="max-width: 100px;" class="d-flex align-items-start" v-if="!user.verificationStatus1"><VueIdentityPass :options="['bvn']" :secretKey="'bb6021e3ac1741318a0c305fe6ba548348730'" reference="hdjidjud" @completed="verification1Completed" /></p>
            </div>

            <div class="col-md-12 mt-4">
                <h4 class="text-white">Level 2 verification (Multiple verification)</h4>
            </div>
            <div class="col-md-12" style="color:#788898fa">
                <p>
                    <span class="mr-3">Verify your NIN and Drivers license</span>
                    <span class="text-danger font-weight-bold" v-if="!user.verificationStatus2">Undone</span>
                    <span class="text-success font-weight-bold" v-else>Done</span>
                </p>
                <p  style="max-width: 100px;"  class="d-flex align-items-start" v-if="!user.verificationStatus2" ><VueIdentityPass :options="['nin,drivers_license']" :secretKey="'bb6021e3ac1741318a0c305fe6ba548348730'" reference="hdjidjud" @completed="verification2Completed"  /></p>
            </div>
        </div>
    </div>
</template>

<script>
import { ref } from '@vue/reactivity';
import VueIdentityPass from 'vue-v3-identitypass';
import { useRouter } from "vue-router"

export default {
    components: {
        VueIdentityPass
    },

    setup() {
        const router = useRouter();

        const verification1Completed = data => {
            const { status } = data;

            if (status) {
                const users = JSON.parse(localStorage.getItem('users'));
                const indexOfUser = users.findIndex(i => i.id === +userId) ?? { };

                users[indexOfUser].verificationStatus1 = true;
                localStorage.setItem('users', JSON.stringify(users));
                user.value = users[indexOfUser];
            }
        }

        const verification2Completed = data => {
            const { status } = data;

            if (status) {
                const users = JSON.parse(localStorage.getItem('users'));
                const indexOfUser = users.findIndex(i => i.id === +userId) ?? { };

                users[indexOfUser].verificationStatus2 = true;
                localStorage.setItem('users', JSON.stringify(users));
                user.value = users[indexOfUser];
            }
        }

        const user = ref({ })

        const userId = localStorage.getItem('token');

        if (!userId) router.push('/');
        const users = JSON.parse(localStorage.getItem('users'));

          if (!users) {
              router.push('/')
              return false;
          } else {
              
              user.value = users.find(i => i.id === +userId) ?? { };
              if (!user.value) {
                  router.push('/')
              }
          }

        return {
            verification1Completed,
            verification2Completed,
            user,
        }
    },
}
</script>

<style scoped>
    button {
        margin: 0 !important;
    }
</style>
