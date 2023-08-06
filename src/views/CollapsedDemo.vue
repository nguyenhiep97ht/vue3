<template >
    <n-collapse-transition :show="show">
        ahuhu
        <div v-for="item in data" :key="item.id">
            {{ item.name }}
        </div>
    </n-collapse-transition>
</template>
<script setup lang="ts">
import { ref, watchEffect } from 'vue';

const props = defineProps<{
    show: boolean
}>()
const isCalled = ref<boolean>(false)
const data = ref()

const fetchData = async () => {
    const res = await fetch('http://localhost:8080/api/products', {
        method: "POST",
        headers: {
            "Content-Type": "application/json",
        },
        body: JSON.stringify({
            limit: 10,
            offset: 0
        })
    }).then((data) => {
        return data.json()
    })
    return res
}

watchEffect(async () => {
    console.log('`ahuhu` :>> ', `ahuhu`);
    if (props.show && !isCalled.value) {
        console.log('`called` :>> ', `called`);
        const res = await fetchData()
        data.value = res.content
        isCalled.value = true
    }
})
</script>
<style lang="">
    
</style>