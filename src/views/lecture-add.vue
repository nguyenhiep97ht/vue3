<template>
    <n-h5>{{ section.name }}</n-h5>
    <ul>
        <template v-if="value">
            <li v-for="lec in value.find(item => item.id === section.id)?.children" :key="lec.id">
                {{ lec.name }}
            </li>
        </template>

        <li>
            <SelectPopover button-text="select lectures" :options="lectureOptions ?? []"
                v-model:selectedIds="selectedIds" />
        </li>
    </ul>
</template>

<script setup lang="ts">
import { computed, ref, watch } from 'vue';
import SelectPopover from './select-popover.vue';
import { useVModel } from '@vueuse/core'

const props = defineProps<{
    section: any
    options?: any[]
    value: { id: string; children: any[] }[]
}>()
const selectedIds = ref([])

const emit = defineEmits(['update:value'])
const value = useVModel(props, 'value', emit)

watch(selectedIds, () => {
    const selected: any[] = [];
    selectedIds.value.forEach((id: string) => {
        const existed = props.options?.find(opt => opt.id === id)
        if (existed) selected.push(existed)
    })
    const x = value.value.find(item => item.id === props.section.id)
    if (x) x.children = selected
    else value.value = [...value.value, { id: props.section.id, children: selected }]
})

const lectureOptions = computed(() => {
    return props.options?.map(item => ({ label: item.name, value: item.id }))
})
</script>

<style scoped></style>