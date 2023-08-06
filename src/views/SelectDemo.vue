<template >
    <n-space vertical>
        <n-button icon-placement="right" type="primary" ghost @click="onSelectAllSecsAndLects">
            <template #icon>
                <span>
                    <n-checkbox :checked="isCheckedAll" />
                </span>
            </template>
            Select Sections and Lectures
        </n-button>

        <SelectPopover buttonText="select Sections" :options="sectionOptions" v-model:selectedIds="selectedSectionIds"
            :disabled="btnDisabled" />

        <div v-for="sec in (selectedSections)" :key="sec.id">
            <LectureAdd v-model:value="selectedLectures" :section="sec"
                :options="Sections.find(item => item.id === sec.id)?.lectures" />
        </div>

    </n-space>
</template>
<script setup lang="ts">
import { computed, ref } from 'vue';
import Sections from './demo'
import SelectPopover from './select-popover.vue';
import LectureAdd from './lecture-add.vue';

const isCheckedAll = ref<boolean>(false)

const btnDisabled = ref<boolean>(false)
const sections = ref(Sections.map(item => ({ ...item, lectures: [] })))
const sectionOptions = computed(() => {
    return sections.value.map(item => ({ value: item.id, label: item.name }))
})
const selectedSectionIds = ref<string[]>([])
const selectedLectures = ref<{ id: string; children: any[] }[]>([])

const selectedSections = computed(() => {
    if (isCheckedAll.value) return Sections
    const selected: any[] = [];
    selectedSectionIds.value.forEach(id => {
        const existSection = sections.value.find(section => section.id === id)
        if (existSection) selected.push(existSection)
    })
    return selected
})

const onSelectAllSecsAndLects = () => {
    isCheckedAll.value = !isCheckedAll.value
    selectedSectionIds.value = []
    btnDisabled.value = isCheckedAll.value
    selectedLectures.value = isCheckedAll.value ? Sections.map(item => ({ id: item.id, children: item.lectures })) : []
}


</script>
<style lang="">
    
</style>