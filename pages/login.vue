<template>
    <div id="AuthPage" class="w-full h-[100vh] bg-white overflow-hidden">
        <div
            class="w-full flex items-start justify-center p-5 border-b border-b-gray-300"
        >
            <NuxtLink to="/" class="min-w-[170px]">
                <img src="/AliExpress-logo.png" width="170" alt="" />
            </NuxtLink>
        </div>
        <div
            class="flex items-center flex-col h-[80vh] justify-center max-w-[400px] mx-auto px-2"
        >
            <div class="text-center my-6">Login / Register</div>
            <button
                class="w-full flex items-center justify-center gap-3 p-1.5 border hover:bg-gray-100 rounded-full text-lg font-semibold"
                @click="login('google')"
            >
                <img src="/google-logo.png" class="max-w-[20px]" alt="" />
                <div>Google</div>
            </button>

            <button
                class="mt-4 w-full flex items-center justify-center gap-3 p-1.5 border hover:bg-gray-100 rounded-full text-lg font-semibold"
                @click="login('github')"
            >
                <img src="/github-logo.png" class="max-w-[20px]" alt="" />
                <div>Github</div>
            </button>
        </div>
    </div>
</template>

<script setup>
const client = useSupabaseClient();
const user = useSupabaseUser();

watchEffect(() => {
    if (user.value) {
        return navigateTo("/");
    }
});

const login = async (provider) => {
    const { data, error } = await client.auth.signInWithOAuth({
        provider: provider,
    });
};
</script>
