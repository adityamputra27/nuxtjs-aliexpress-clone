<template>
    <div
        :id="`Product${product.id}`"
        class="bg-white inline-block rounded hover:shadow-[0_0_10px_3px_rgba(0,0,0,0.15)] cursor-pointer"
    >
        <NuxtLink :to="`/item/${product.id}`">
            <img :src="product.url" alt="" />
            <div id="ProductDetails">
                <span class="flex items-center justify-start gap-3 px-1 pt-1">
                    <span class="text-[#FF6674] font-semibold"
                        >${{ priceComputed }}</span
                    >
                    <span class="text-gray-500 text-sm text-light line-through"
                        >${{ oldPriceComputed }}</span
                    >
                </span>
                <span
                    class="px-1 relative -top-1.5 text-[#FF6674] text-xs font-semibold"
                    >Extra 5% off</span
                >
                <div class="flex items-center gap-1 px-1 relative -top-1">
                    <span
                        class="text-white bg-[#FD374F] text-[9px] font-semibold px-1.5 rounded-sm"
                        >Welcome Deal!</span
                    >
                    <span
                        class="bg-[#F5F5F5] border text-[#C08562] text-[9px] font-semibold px-1.5 rounded-sm"
                        >Top Selling</span
                    >
                </div>

                <p class="flex items-center px-1 pt-0 5 text-xs text-[#252525]">
                    5,000+ sold
                    <Icon
                        name="material-symbols:star-rate"
                        class="ml-1.5"
                        color="#757575"
                    />
                    4.7
                </p>

                <p class="px-1 pt-0.5 text-xs text-[#252525]">
                    {{ product.title }}
                </p>

                <p class="px-1 pb-1">
                    <span class="text-[#009A66] text-xs font-semibold"
                        >Free Shipping</span
                    >
                </p>
            </div>
        </NuxtLink>
    </div>
</template>
<script setup lang="ts">
import { defineProps } from "vue";
const props = defineProps(["product"]);

const { product } = props;

const priceComputed = computed(() => {
    return product.price / 100;
});

const oldPriceComputed = computed(() => {
    let res = (product.price + product.price / 20) / 100;
    return res.toFixed(2);
});
</script>
