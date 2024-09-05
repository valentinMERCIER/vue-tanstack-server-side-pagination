<template>
  <div class="m-2 p-2 border">
    <h2 class="text-lg">My Data Table component</h2>
    <div class="m-1 p-1 border">
      <h3 class="underline">table.getRowModel().rows</h3>
      <div v-for="row in table.getRowModel().rows">
        {{ row }}
      </div>
    </div>
    <ClientOnly>
      <div class="flex items-center justify-end py-4 space-x-2 mr-4">
        <button variant="outline" size="xs" :disabled="!table.getCanPreviousPage()" @click="previousPage">
          Previous
        </button>
        <button variant="outline" size="xs" :disabled="!table.getCanNextPage()" @click="nextPage">
          Next
        </button>
      </div>
    </ClientOnly>
    <div>
      <strong>props.data:</strong> {{ props.data }}
    </div>
  </div>
</template>

<script lang="ts" setup generic="TData, TValue">
import type { ColumnDef, PaginationState, TableOptions } from '@tanstack/vue-table'
import {
  FlexRender,
  getCoreRowModel,
  useVueTable,
} from '@tanstack/vue-table'


const props = defineProps<{
  columns: ColumnDef<TData, TValue>[],
  data: TData[],
  totalRowCount: number,
  pagination: PaginationState
}>()

const table = useVueTable({
  get data() { return props.data },
  get columns() { return props.columns },
  getCoreRowModel: getCoreRowModel(),
  manualPagination: true,
  rowCount: props.totalRowCount,
  debugAll: true,
  initialState: {
    pagination: props.pagination
  }

});

watch(() => props.totalRowCount, () => {
  table.setOptions(prev => ({
    ...prev,
    rowCount: props.totalRowCount
  }))
})

const emit = defineEmits(['update:pagination'])

watch(() => table.getState().pagination, () => {
  emit('update:pagination', table.getState().pagination)
})

const previousPage = () => {
  if (table.getCanPreviousPage()) {
    table.previousPage()
  }
}

const nextPage = () => {
  if (table.getCanNextPage()) {
    table.nextPage()
  }
}
</script>

<style></style>