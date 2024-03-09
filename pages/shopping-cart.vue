<template>
    <NuxtLayout>
        <div id="ShoppingCartPage" class="mt-4 max-w-[1200px] mx-auto px-2">
            <div
                v-if="!userStore.cart.length"
                class="h-[500px] flex items-center justify-center"
            >
                <div class="pt-20">
                    <img
                        src="/cart-empty.png"
                        class="mx-auto"
                        width="250"
                        alt=""
                    />
                    <div class="text-xl text-center mt-4">No items yet?</div>
                    <div v-if="!user" class="flex text-center">
                        <NuxtLink
                            to="/auth"
                            class="bg-[#FD374F] w-full text-white text-[21px] font-semibold p-1.5 rounded-full mt-4"
                            >Sign in</NuxtLink
                        >
                    </div>
                </div>
            </div>
            <div class="md:flex gap-4 justify-between mx-auto w-full" v-else>
                <div class="md:w-[65%]">
                    <div class="bg-white rounded-lg p-4">
                        <div class="text-2xl font-bold mb-2">
                            Shopping Cart ({{ userStore.cart.length }})
                        </div>
                    </div>
                    <div class="bg-[#FEEEEEF] rounded-lg p-4 mt-4">
                        <div class="text-red-500 font-bold">
                            Welcome Deal applicable on 1 item only
                        </div>
                    </div>

                    <div id="Items" class="bg-white rounded-lg p-4 mt-4">
                        <div
                            v-for="(product, index) in userStore.cart"
                            :key="index"
                        >
                            <CartItem
                                :product="product"
                                :selectedArray="selectedArray"
                                @selectedRadio="selectedRadioFunc"
                            />
                        </div>
                    </div>
                </div>
                <div class="md:w-[35%]">
                    <div id="Summary" class="bg-white rounded-lg p-4">
                        <div class="text-2xl font-extrabold mb-2">Summary</div>
                        <div class="flex items-center justify-between my-4">
                            <div class="font-semibolld">Total</div>
                            <div class="text-2xl font-semibold">
                                $
                                <span class="font-extrabold">{{
                                    totalPriceComputed
                                }}</span>
                            </div>
                        </div>
                        <button
                            class="flex items-center justify-center bg-[#FD364F] w-full text-white text-[21px] font-semibold p-1.5 rounded-full mt-4"
                            @click="goToCheckout"
                        >
                            Checkout
                        </button>
                    </div>
                    <div
                        id="PaymentProtection"
                        class="bg-white rounded-lg p-4 mt-4"
                    >
                        <div class="text-lg font-semibold mb-2">
                            Payment methods
                        </div>
                        <div class="flex items-center justify-start gap-8 my-4">
                            <div v-for="(card, index) in cards" :key="index">
                                <img :src="card" alt="" class="h-6" />
                            </div>
                        </div>
                    </div>
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

let selectedArray = ref([]);
let userStore = useUserStore();
let user = useSupabaseUser();

onBeforeMount(() => {
    setTimeout(() => (userStore.isLoading = false), 1000);
});

const totalPriceComputed = computed(() => {
    let price = 0;
    userStore.cart.forEach((product) => {
        price += product.price;
    });
    return price / 100;
});

const selectedRadioFunc = (e) => {
    if (!selectedArray.value.length) {
        selectedArray.value.push(e);
        return;
    }

    selectedArray.value.forEach((item, index) => {
        if (e.id != item.id) {
            selectedArray.value.push(e);
        } else {
            selectedArray.value.splice(index, 1);
        }
    });
};

const goToCheckout = () => {
    let ids = [];
    userStore.checkout = [];

    selectedArray.value.forEach((item) => ids.push(item.id));

    let res = userStore.cart.filter((item) => {
        return ids.indexOf(item.id) != -1;
    });

    res.forEach((item) => userStore.checkout.push(toRaw(item)));

    return navigateTo("/checkout");
};

const cards = ref(["visa.png", "mastercard.png", "paypal.png", "applepay.png"]);
</script>
