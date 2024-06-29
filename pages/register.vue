<template>
  <div>
    <label>email</label>
    <br>
    <input type="email" v-model="email" />
    <br>
    <br>
    <label>password</label>
    <br>
    <input type="password" v-model="password" />
    <br>
    <br>
    <label>username</label>
    <br>
    <input type="text" v-model="username" />
    <br>
    <br>

    <label>address</label>
    <br>
    <input type="text" v-model="address" />
    <br>
    <br>
    <div v-if="!successMsg">
      <button @click="signUp">Sign up</button>
    </div>
    <div v-else>
      <p v-if="errorMsg">Error!!</p>
      <p v-else>Success!!</p>
    </div>
  
  </div>
</template>

<script setup lang="ts">
  const supabase = useSupabaseClient();
  const user = useSupabaseUser();
  const email = useState(() => "");
  const password = useState(() => "");
  const username = useState(() => "");
  const address = useState(() => "");
  const successMsg = useState(() => false);
  const errorMsg = useState(() => false);

  if (user.value) await navigateTo("/dashboard");

  const signUp = async () => {
    const { data, error } = await supabase.auth.signUp({
      email: email.value,
      password: password.value,
      options: {
        data: {
          username: username.value,
          address: address.value,
        },
        emailRedirectTo: "http://localhost:3000/dashboard",
      },
    });
    if(error) {
      successMsg.value = false
      errorMsg.value = true
      return
    }

    errorMsg.value = false
    successMsg.value = true
    setTimeout(async ()=>{
      successMsg.value = false
      await navigateTo('/confirm')
    },2000)
  };
</script>

<style scoped></style>
