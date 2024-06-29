<template>
    <div>
        <h1>User Dashboard</h1>
        <br>
        <button @click="logout">Logout</button>
    </div>
</template>

<script setup lang="ts">
    const supabase = useSupabaseClient()
    const user = useSupabaseUser()
    const errorMsg = useState(()=>"")
    const successMsg = useState(()=>"")
    const logout = async () => {
        const {error} = await supabase.auth.signOut()
        if(error){
            errorMsg.value = error.message
            return
        }

        successMsg.value = "You logged out successfully!"
        setTimeout(async()=>{
            successMsg.value = ""
            await navigateTo('/')
        },2000)
    }

definePageMeta({
    middleware:[
        async ()=> {
            const user = useSupabaseUser()
            if(!user){
                return navigateTo('/login')
            }
        }
    ]
})
</script>
