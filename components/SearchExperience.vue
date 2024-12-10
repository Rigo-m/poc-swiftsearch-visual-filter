<template>
  <div>
    <AisInstantSearch :widgets :configuration>
      <AisInfiniteHits>
        <template #default="{ items }">
          <div v-for="item in items" :key="item.objectID">{{ item.brand }}</div>
        </template>
      </AisInfiniteHits>
      <AisRefinementList attribute="brand" searchable />
      <VisualFilter />
    </AisInstantSearch>
  </div>
</template>

<script setup lang="ts">
import algoliasearch from "algoliasearch";

const client = algoliasearch("latency", "6be0576ff61c053d5f9a3225e2a90f76", {
});

const widgets = computed(() => [
  useAisInfiniteHits({
    showPrevious: true,
  }),
  useAisRefinementList({
    attribute: "brand",
    showMore: true,
    limit: 10,
    showMoreLimit: 20,
    sortBy: ["isRefined", "name:asc"]
  }),
]);

const configuration = ref({
  indexName: "instant_search",
  searchClient: client,
});
</script>
