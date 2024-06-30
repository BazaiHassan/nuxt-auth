<template>
  <div class="flex flex-row justify-center h-screen bg-slate-100 items-center" >
  <Card class="w-[350px]">
    <CardHeader>
      <CardTitle>Create Account</CardTitle>
      <CardDescription>Create account to interact with our services.</CardDescription>
    </CardHeader>
    <CardContent>
        <div class="grid items-center w-full gap-4">
          <div class="flex flex-col space-y-6">
          
            <Input   type="email" placeholder="Email Address" v-model="email"/>
            
            <Input   type="password" placeholder="Password" v-model="password"/>

            <Input   type="text" placeholder="Username" v-model="username"/>

            <Input   type="text" placeholder="Address" v-model="address"/>

          </div>
        </div>
    </CardContent>
    <CardFooter class="flex justify-between px-6 pb-6">
      <Button @click="signUp" class="w-full">Register</Button>
    </CardFooter>
  </Card>
</div>
</template>


<script setup lang="ts">
import { useToast } from '@/components/ui/toast/use-toast'

  const supabase = useSupabaseClient();
  const user = useSupabaseUser();
  const email = useState(() => "");
  const password = useState(() => "");
  const username = useState(() => "");
  const address = useState(() => "");
  const successMsg = useState(() => false);
  const errorMsg = useState(() => false);

  const {toast} = useToast()

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
      toast({
        title: 'Opps',
        description: 'Login failed, please try again.',
        variant: 'destructive',
      });
      return
    }

    toast({
        title: 'Horay',
        description: 'Login Successfull, enjoy.',
      });

    errorMsg.value = false
    successMsg.value = true
    setTimeout(async ()=>{
      successMsg.value = false
      await navigateTo('/confirm')
    },2000)
  };
</script>

<style scoped></style>
