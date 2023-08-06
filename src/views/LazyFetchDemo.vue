<template>
    <div v-for="item in data" :key="item.id">
        {{ item.name }}
    </div>
</template>
<script setup lang="ts">
import { onMounted, ref } from 'vue';


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

onMounted(async () => {
    const res = await fetchData()
    data.value = res.content
})
</script>
<style lang="">
    
</style>