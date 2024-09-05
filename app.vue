<template>
  <div class="m-3 p-3 border">
    <h1 class="text-xl">My Parent page </h1>
    <div>
      totalCount: {{ totalCount }}
    </div>
    <div>
      currentPage: {{ currentPage }}
    </div>
    <div>
      dataCurrentPage: {{ dataCurrentPage }}
    </div>
    <div>
      <DataTable :columns="columns" :data="dataCurrentPage" :totalRowCount="totalCount" :pagination="{
        pageIndex: currentPage - 1, pageSize
      }" @update:pagination="handlePaginationUpdate" />

    </div>
  </div>
</template>

<script lang="ts" setup>

const allData = [
  { id: 1, name: 'John1' },
  { id: 2, name: 'John2' },
  { id: 3, name: 'John3' },
  { id: 4, name: 'John4' },
  { id: 5, name: 'John5' },
  { id: 6, name: 'John6' },
  { id: 7, name: 'John7' },
  { id: 8, name: 'John8' },
  { id: 9, name: 'John9' },
]

const currentPage = ref(1);
const pageSize = ref(3);
const totalCount = ref(0)

const columns = [
  { accessorKey: 'id', Header: 'ID' },
  { accessorKey: 'name', Header: 'Name' }
]

const dataCurrentPage = ref([]);

const fetchData = () => {
  const offset = (currentPage.value - 1) * pageSize.value;

  totalCount.value = allData.length

  dataCurrentPage.value = allData.slice(offset, offset + pageSize.value)
}


onMounted(() => {
  fetchData();
});

const handlePaginationUpdate = (pagination: PaginationState) => {
  currentPage.value = pagination.pageIndex + 1
  pageSize.value = pagination.pageSize
}

watch([currentPage], fetchData);

</script>

<style></style>