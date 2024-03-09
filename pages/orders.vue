<template>
    <NuxtLayout>
        <div
            id="OrdersPage"
            class="mt-4 max-w-[1200px] mx-auto px-2 min-h-[50vh]"
        >
            <div class="bg-white w-full p-6 min-h-[150px]">
                <div class="flex items-center text-xl-">
                    <Icon name="carbon:delivery" color="#5FCB04" size="35" />
                    <span class="pl-4">Orders</span>
                </div>
                <div
                    v-if="orders && orders.data"
                    v-for="(order, index) in orders.data"
                    :key="index"
                    class="text-sm pl-[50px]"
                >
                    <div class="border-b pt-3 pb-1">
                        <p>Stripe ID : {{ "-" }}</p>
                        <p>
                            Order Date :
                            {{ formattedDate(order.created_at) }}
                        </p>
                        <div class="pt-2"></div>
                        <div
                            v-for="(orderItem, index) in order.orderItem"
                            :key="index"
                        >
                            <NuxtLink
                                :to="`/item/${orderItem.productId}`"
                                class="flex items-center gap-3 p-1 hover:underline hover:text-blue-500"
                            >
                                <img
                                    :src="orderItem.product.url"
                                    class=""
                                    width="40"
                                    alt=""
                                />
                                {{ orderItem.product.title }}
                            </NuxtLink>
                        </div>
                        <div class="pt-2 pb-5">
                            Delivery Address: {{ order.name }},
                            {{ order.address }}, {{ order.zipcode }},
                            {{ order.city }}, {{ order.country }}
                        </div>
                    </div>
                </div>
                <div v-else class="flex items-center justiy-center">
                    You have no order history
                </div>
            </div>
        </div>
    </NuxtLayout>
</template>
<script setup>
definePageMeta({
    layout: "main-layout",
});

import { useUserStore } from "~/stores/user";
const userStore = useUserStore();
const user = useSupabaseUser();

let orders = ref(null);

onBeforeMount(async () => {
    orders.value = await useFetch(
        `/api/prisma/get-all-orders-by-user/${user.value.id}`
    );
});

const formattedDate = (timestamp) => {
    const date = new Date(timestamp);
    const formatter = new Intl.DateTimeFormat("id-ID", {
        month: "2-digit",
        day: "2-digit",
        year: "numeric",
        hour: "2-digit",
        minute: "2-digit",
        second: "2-digit",
    });
    const result = formatter.format(date);

    return result;
};

onMounted(() => {
    if (!user.value) {
        return navigateTo("/login");
    }

    setTimeout(() => (userStore.isLoading = false), 200);
});
</script>
