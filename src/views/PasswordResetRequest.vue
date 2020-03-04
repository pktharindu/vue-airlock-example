<template>
  <div class="min-h-screen flex items-center justify-center bg-gray-50 py-12 px-4 sm:px-6 lg:px-8">
    <div class="max-w-md w-full">
      <div>
        <img
          class="mx-auto h-12 w-auto"
          src="../assets/logo.png"
          alt="Workflow"
        />
        <h2 class="mt-6n text-center text-3xl leading-9 font-extrabold text-gray-900">
          Laravel Airlock
        </h2>
        <p class="mt-2 text-center text-sm leading-5 text-gray-600 max-w">
          Password Reset
        </p>
      </div>
      <form
        class="mt-8"
        action="#"
        method="POST"
        @submit.prevent="login"
      >
        <div
          v-if="errorMessage"
          class="text-red-500 text-sm mb-4"
        >{{ errorMessage }}</div>

        <div
          v-if="successMessage"
          class="text-green-500 text-sm mb-4"
        >{{ successMessage }}</div>

        <div class="rounded-md shadow-sm">
          <div>
            <input
              v-model="email"
              aria-label="Email address"
              name="email"
              type="email"
              required
              class="appearance-none relative block w-full px-3 py-2 border border-gray-300 placeholder-gray-500 text-gray-900 rounded-md focus:outline-none focus:shadow-outline-blue focus:border-blue-300 focus:z-10 sm:text-sm sm:leading-5"
              placeholder="Email address"
            />
          </div>
        </div>

        <div class="mt-6">
          <button
            type="submit"
            class="group relative w-full flex justify-center py-2 px-4 border border-transparent text-sm font-medium rounded-md text-white bg-indigo-600 hover:bg-indigo-500 focus:outline-none focus:border-indigo-700 focus:shadow-outline-indigo active:bg-indigo-700 transition duration-150 ease-in-out"
          >
            <span class="absolute left-0 inset-y pl-3">
              <svg
                class="h-5 w-5 text-indigo-500 group-hover:text-indigo-400 transition ease-in-out duration-150"
                fill="currentColor"
                viewBox="0 0 20 20"
              >
                <path
                  fill-rule="evenodd"
                  d="M5 9V7a5 5 0 0110 0v2a2 2 0 012 2v5a2 2 0 01-2 2H5a2 2 0 01-2-2v-5a2 2 0 012-2zm8-2v2H7V7a3 3 0 016 0z"
                  clip-rule="evenodd"
                />
              </svg>
            </span>
            Send Password Reset Link
          </button>
        </div>
      </form>
    </div>
  </div>
</template>

<script>
// @ is an alias to /src
import axios from "axios";

axios.defaults.withCredentials = true;
axios.defaults.baseURL = "http://controlspace.test";

export default {
  name: "Home",
  data() {
    return {
      email: "",
      errorMessage: "",
      successMessage: ""
    };
  },
  methods: {
    login() {
      axios.get("/airlock/csrf-cookie").then(response => {
        // console.log(response)
        axios
          .post("/password/email", {
            email: this.email
          })
          .then(response2 => {
            // console.log(response2.data.message);
            this.successMessage = response2.data.message;
          })
          .catch(error => {
            // console.log(error.response.data);
            const key = Object.keys(error.response.data.errors)[0];
            this.errorMessage = error.response.data.errors[key][0];
          });
      });
    }
  }
};
</script>
