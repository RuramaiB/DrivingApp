<template>
  <div>
    <nav
    class="relative w-full flex flex-wrap items-center justify-between py-3 bg-green-900 text-gray-200 shadow-lg navbar navbar-expand-lg navbar-light">
    <div class="container-fluid w-full flex flex-wrap items-center justify-between px-6">
      <div class="collapse navbar-collapse flex-grow items-center" id="navbarSupportedContent1">
        <a class="text-xl text-white pr-2 font-semibold" href="#">Settings</a>
    
      </div>
      <!-- Collapsible wrapper -->
  
      <!-- Right elements -->
      <div class="relative my-1 z-10">
        <button
          @click="isOpen = !isOpen"
          class="inline-flex items-center justify-center  font-bold text-md  text-white border-none"
          aria-expanded="false"
        >
          {{ user.email }}
          <svg
            class="-mr-1 ml-2 h-5 w-5"
            xmlns="http://www.w3.org/2000/svg"
            viewBox="0 0 20 20"
            fill="currentColor"
            aria-hidden="true"
          >
            <path
              fill-rule="evenodd"
              d="M6 8l4 4 4-4H6z"
              clip-rule="evenodd"
            ></path>
          </svg>
        </button>
        <div
          v-if="isOpen"
          class="origin-top-right absolute right-0 mt-2 w-56 rounded-md shadow-lg bg-white ring-1 ring-black ring-opacity-5 focus:outline-none"
          role="menu"
          aria-orientation="vertical"
          aria-labelledby="options-menu"
        >
          <div class="py-1 px-2" role="none">
            <!-- Dropdown options -->
            <NuxtLink class=" text-black flex flex-cols text-md font-lg cursor-pointer my-3 gap-2" to=".././settings">
              <svg xmlns="http://www.w3.org/2000/svg" height="20" width="20" viewBox="0 0 24 24"> <g> <path fill="none" d="M0 0h24v24H0z"/> <path d="M12 22C6.477 22 2 17.523 2 12S6.477 2 12 2a9.985 9.985 0 0 1 8 4h-2.71a8 8 0 1 0 .001 12h2.71A9.985 9.985 0 0 1 12 22zm7-6v-3h-8v-2h8V8l5 4-5 4z"/> </g> </svg>
              Logout
            </NuxtLink>
          </div>
        </div>
      </div>
      <!-- Right elements -->
    </div>
  </nav>
  <div class="flex">
    <div class="flex flex-col justify-between w-50 py-20  px-4 min-h-full">
        <div class="flex flex-col flex-auto py-10">
            <div class="p-2 hover:cursor-pointer">
                <div class="flex flex-row space-x-3 py-2 ">
                    <NuxtLink  class="font-medium text-gray-600 cursor-pointer hover:text-black" to="instructor/dashboard/settings/">My Profile</NuxtLink>
                </div>
                
            </div>
            <div class="p-2 hover:cursor-pointer">
              <div class="flex flex-row space-x-3 py-2 ">
                  <NuxtLink  class="font-medium text-gray-600 cursor-pointer hover:text-black" to="instructor/dashboard/settings/notifications">Notifications</NuxtLink>
              </div>
              
          </div>
        <div class="p-2 hover:cursor-pointer">
          <div class="flex flex-row space-x-3 py-2 ">
              <NuxtLink  class="font-medium text-gray-600 cursor-pointer hover:text-black" to="instructor/dashboard/settings/appearances">Appearances</NuxtLink>
          </div>
      </div>
        </div>
    </div>
    <div class="flex-grow relative min-w-screen ">
        <nuxt-child></nuxt-child>
    </div>
</div>
</div>
</template>


<script>
import axios from 'axios'
export default {
  data() {
    return {
      isOpen: false,
      user:[]
    };
    
  },
  methods: {
    async getOrganization(){
    const email = localStorage.email.replace('%40', '@');
    this.loading = true;
    const URL = "http://localhost:8000/getOrganisation/{email}?organization_email=info%40ring.co.zw";
    // const URL = "http://localhost:8000/getOrganisation/{email}?orgianization_email=" + email;
    await axios.get(URL,{
      headers: { "Content-Type": "application/json",
              Authorization : 'Bearer ' + localStorage.token,
            // 'Access-Control-Allow-Origin':'*'
           },
    }).then((res) =>
     {
      this.user = res.data;
      console.log(this.user);

      console.log("info received")
      console.log("Fetching  user Data Completed...");
    }) .catch(error => {
      console.log(error.code)
      this.error=error.code;
      this.errored = true

    }).finally(() => this.loading = false);
  },
  },
  mounted(){
    this.getOrganization()
  }
};
</script>

<style scoped>

</style>