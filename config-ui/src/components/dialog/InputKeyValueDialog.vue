<script setup lang="ts" generic="T extends { [key: string]: any }">
import { useConfigStore } from "@/store/config";
import { ref } from "vue";

const { translate } = useConfigStore()
const props = defineProps<{
  title?: string
  dataObj: Array<T>
}>()

const emit = defineEmits(["save", "add"])
const dataList = ref(props.dataObj)

const dialog = ref(false)

const closeDialog = () => {
  dialog.value = false
}

const resetDataList = () => {
  dataList.value = JSON.parse(JSON.stringify(props.dataObj))
}

const save = () => {
  emit('save', dataList.value)
  closeDialog()
}

</script>

<template>
  <v-dialog v-model="dialog" @update:modelValue="resetDataList" max-width="1400px">
    <template v-slot:activator="{ props }">
      <slot :props="props"></slot>
    </template>

    <v-card>
      <v-card-text class="text-sm-h5 font-weight-bold">{{ props.title }}</v-card-text>

      <div class="ml-6">
        <slot name="tips"></slot>
      </div>

      <v-card-text>
        <v-row class="font-weight-bold" :dense="true">
          <slot name="contentsTitle"></slot>
        </v-row>
        <v-row v-for="(data, index) in dataList" :key="index" :dense="true">
          <slot name="contents" :data="data"></slot>
        </v-row>
      </v-card-text>
      <v-card-actions class="justify-end">
        <slot name="otherActions"></slot>
        <v-btn class="text-none" color="green" variant="flat" density="default" @click="emit('add', dataList)">{{ translate('label:609') }}</v-btn>
        <v-btn class="text-none" color="blue" variant="flat" density="default" @click="save">{{ translate('label:610') }}</v-btn>
        <v-btn class="text-none" color="grey" variant="flat" density="default" @click="closeDialog">{{ translate('label:611') }}</v-btn>
      </v-card-actions>
    </v-card>

  </v-dialog>
</template>

<style scoped>
.v-row:first-child :slotted(.v-col) {
  padding-left: 10px;
}
</style>
