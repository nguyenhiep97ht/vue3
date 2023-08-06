<script setup lang="ts">
import { ref } from 'vue';
import LazyFetchDemo from './LazyFetchDemo.vue';
import CollapsedDemo from './CollapsedDemo.vue';
import SelectDemo from './SelectDemo.vue';

const show = ref<boolean>(false)
</script>

<template>
  <main>
    <n-card title="Song of" style="margin-bottom: 16px">
      <n-tabs type="line" animated>
        <n-tab-pane name="oasis" tab="Oasis" display-directive="show:lazy">
          Wonderwall
        </n-tab-pane>
        <n-tab-pane name="the beatles" tab="the Beatles" display-directive="show:lazy">
          Hey Jude
        </n-tab-pane>
        <n-tab-pane name="jay chou" tab="Jay Chou" display-directive="show:lazy">
          <LazyFetchDemo />
        </n-tab-pane>
      </n-tabs>
    </n-card>

    <n-card title="Card Segmented Demo" style="margin-bottom: 16px">
      <template #header-extra>
        <span @click="show = !show">
          expanded/collapsed
        </span>
      </template>
      <div>
        <CollapsedDemo :show="show" />
      </div>

      <template #footer>
        #footer
      </template>
      <template #action>
        #action
      </template>
    </n-card>

    <n-card>
      <SelectDemo />
    </n-card>
  </main>
</template>
