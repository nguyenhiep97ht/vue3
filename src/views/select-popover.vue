<template>
    <n-popover :show="showPopover" placement="bottom" trigger="manual" @clickoutside="showPopover = !showPopover">
        <template #trigger>
            <n-button @click="showPopover = !showPopover" :disabled="disabled">
                {{ buttonText }}
            </n-button>
        </template>
        <n-checkbox label="select all" v-model:checked="isCheckedAll" @update:checked="handleSelectAll" />
        <n-checkbox-group v-model:value="selectedIds" @update:value="handleSelect">
            <n-space vertical>
                <n-checkbox v-for="opt in options" :key="opt.value" :value="opt.value" :label="opt.label" />
            </n-space>
        </n-checkbox-group>
    </n-popover>
</template>

<script setup lang="ts">
import { ref, watch } from 'vue';
import { useVModel } from '@vueuse/core'

const props = defineProps<{
    options: { value: string; label: string }[],
    disabled?: boolean,
    selectedIds: string[],
    buttonText: string
}>()

const showPopover = ref<boolean>(false)

const isCheckedAll = ref<boolean>(false)
const emit = defineEmits(['update:selectedIds'])
const selectedIds = useVModel(props, 'selectedIds', emit)

const handleSelect = (value: string[]) => {
    isCheckedAll.value = props.options.length === value.length
}

const handleSelectAll = (value: boolean) => {
    selectedIds.value = value ? props.options.map(item => item.value) : []
}

watch(selectedIds, (current) => {
    if (!current || !current.length) {
        handleSelect([])
        handleSelectAll(false)
    }
})
</script>

<style scoped></style>