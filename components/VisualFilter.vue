<template>

  <div class="flex">
    <div class="item" @click="selectApple" :class="{ selected: isSelected }">
      <img src="https://m.media-amazon.com/images/I/81yP+dpbmeL.jpg" />
      <div>{{ howMany }}</div>
    </div>
  </div>
</template>

<script setup lang="ts">

const { getInstance } = useInstantSearch()

const instance = getInstance()

const { state: _state } = useAisWidget("refinementList")

const state = computed(() => _state.value[
  'brand'
])

const selectApple = () => {
  state.value.refine("Platinum")
}


const appleItem = computed(() => state.value.items.filter(i => i.label === 'Platinum')[0])
const howMany = computed(() => {
  return appleItem.value?.count ?? 100000
})

const isSelected = computed(() => appleItem.value?.isRefined ?? false)

// serach for Platinum facet value
const getPlatinumFacet = async () => await instance.value?.client?.searchForFacetValues?.([{
  indexName: 'instant_search',
  params: {
    facetName: 'brand',
    facetQuery: 'Platinum'
  }
}])
onMounted(async () => {
  const pFacet = await getPlatinumFacet()
  console.log(pFacet)
})
</script>


<style scoped>
.flex {
  display: flex;
  gap: 32px;
  margin: 20px;
}

.item {
  width: 60px;
  height: 60px;
  cursor: pointer;
}

.item.selected {
  border: 3px solid red;
}

img {
  width: 100%;
  height: 100%;
  object-fit: cover;
}
</style>
