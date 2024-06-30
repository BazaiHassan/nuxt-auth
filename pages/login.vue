<template>
  <div>
    <label>email</label>
    <br />
    <input type="email" v-model="email" />
    <br />
    <br />
    <label>password</label>
    <br />
    <input type="password" v-model="password" />

    <div v-if="!successMsg">
      <button @click="login">Login</button>
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
  const successMsg = useState(() => false);
  const errorMsg = useState(() => false);

  if (user.value) await navigateTo("/dashboard");

const login = async () => {
  const {data, error} = await supabase.auth.signInWithPassword({
    email:email.value,
    password:password.value,
  })

  if(error) {
      successMsg.value = false
      errorMsg.value = true
      return
    }

    errorMsg.value = false
    successMsg.value = true
    setTimeout(async ()=>{
      successMsg.value = false
      await navigateTo('/dashboard')
    },2000)
}
</script>
