<template>
  <div>
    <form class="flex flex-col items-center" @submit.prevent="signUp" v-if="!confirmPassword">
      <div class="flex flex-col user">
        <label class="block text-gray-700 text-sm font-bold mb-2" for="userName">User Name</label>
        <input
          class="shadow appearance-none border rounded w-full py-2 px-3 text-gray-700 leading-tight"
          type="text"
          v-model="username"
          id="userName"
        />
      </div>
      <div class="flex flex-col mt-2">
        <label class="block text-gray-700 text-sm font-bold mb-2" for="password">Password</label>
        <input
          class="shadow appearance-none border rounded w-full py-2 px-3 text-gray-700 leading-tight"
          type="password"
          v-model="password"
          id="password"
        />
      </div>
      <div class="flex flex-col mt-2">
        <label class="block text-gray-700 text-sm font-bold" for="email">Email</label>
        <input
          class="shadow appearance-none borde rounded w-full py-2 px-3 text-gray-700 leading-tight"
          type="email"
          v-model="email"
          id="email"
        />
      </div>
      <button class="btn-blue">Sign Up</button>
    </form>
    <div class="text-red-600" v-if="error">{{ error.message }}</div>
    <div class="w-4/12 m-auto" v-if="confirmPassword">
      <h3>Enter your code. Please check your email</h3>
      <div class="flex flex-col mt-2">
        <label for="password" class="block text-gray-700 text-sm font-bold">Code</label>
        <input
          type="text"
          class="shadow appearance-none border rounded w-full py-2 px-3 text-gray-700"
          v-model="code"
        />
        <button class="btn-blue" @click="confirmSignUp">Confirm Code</button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data: () => ({
    username: "",
    password: "",
    email: "",
    error: "",
    confirmPassword: false,
    code: ""
  }),
  methods: {
    async signUp() {
      if (!this.email || !this.password) {
        return;
      }
      try {
        await this.$store.dispatch("auth/signUp", {
          username: this.username,
          password: this.password,
          email: this.email
        });

        this.confirmPassword = true;
      } catch (err) {
        this.error = err;
        console.log(err);
      }
    },
    async confirmSignUp() {
      if (!this.username || !this.code) {
        return;
      }

      try {
        const { username, password, code } = this;
        await this.$store.dispatch("auth/confirmSignUp", {
          username,
          code
        });
        await this.$store.dispatch("auth/login", {
          username,
          password
        });
        this.$router.push("/albums");
      } catch (err) {
        this.error = err;
        console.log(err);
      }
    }
  }
};
</script>

<style lang="scss" scoped>
</style>