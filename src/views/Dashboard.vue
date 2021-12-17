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
            <div class="col-md-12 my-5" style="color:#788898fa" v-if="user.verificationStatus1 && user.verificationStatus2">
                <table>
                    <thead>
                        <tr>
                            <th class="font-weight-bold">Crypto Currency</th>
                            <th class="font-weight-bold"></th>
                            <th class="font-weight-bold">Dollar amount</th>
                        </tr>
                    </thead>
                    <tbody>
                        <tr v-for="(item, index) in list" :key="index">
                            <td>{{ item.name }}</td>
                            <td style="width:100px">&nbsp;</td>
                            <td :class="{ 'text-danger': +item.amount <= 0 }">${{ item.amount }}</td>
                        </tr>
                    </tbody>
                </table>
            </div>
        </div>
    </div>
</template>

<script>
import { computed, ref } from '@vue/reactivity';
import { useRouter } from "vue-router"
import { useStore } from "vuex";

export default {
    setup() {
        const store = useStore();
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

          const rates = computed(() => store.getters.ratesData);

          const list = computed(() => {
              const { success } = rates.value;
              console.log( success, "sttaus");
              if (!success) return [];
              const items = [ ]
              for (let item in rates.value.rates) {
                  let datum = {};
                  datum.name = item;
                  datum.amount = rates.value.rates[item];
                  items.push(datum);
              }
              return items;
          })

          if (!rates.value || !rates.value.success) {
              if (user.value.verificationStatus1 && user.value.verificationStatus2) store.dispatch('getRatesData');
          }

          return {
              user,
              list,
          }
    },
}
</script>
