<script setup lang="ts">
const { client } = useClient();

const { data: tftMapSkins } = await useLocalizedData(async (x) => (await client.tftMapSkins.listAsync({ locale: x, version: "latest"}))
  .sort((a, b) => a.itemId - b.itemId));

const { query, results } = useQueryable(tftMapSkins, (x) => x.itemId, (x) => x.name);
</script>

<template>
  <div class="d-flex flex-column gap-4">
    <div class="d-flex flex-wrap justify-content-end gap-2">
      <Card class="d-flex justify-content-center align-items-center me-auto">
        <span>{{ tftMapSkins.length }} TFT map skins</span>
      </Card>
      <Search v-model="query"/>
    </div>
    <div class="d-flex flex-wrap justify-content-center gap-4">
      <div style="width: 350px;" v-for="tftMapSkin in results" :key="`${tftMapSkin.itemId}`"
        data-aos="zoom-out"
        data-aos-duration="500">
        <div class="ratio ratio-4x3 position-relative trans-hover-grow">
          <LLazyImg class="object-fit-cover rounded" img-class="object-fit-cover rounded" :src="tftMapSkin.getLoadoutsIcon('latest')"/>
          <div class="position-absolute z-1 d-flex flex-column justify-content-end">
            <div class="d-inline-flex justify-content-between align-items-end bg-dark-gradient p-2 rounded">
              <div class="d-flex flex-column">
                <span>{{ tftMapSkin.name }}</span>
                <span class="text-muted">{{ tftMapSkin.groupName }}</span>
              </div>
              <span>{{ tftMapSkin.itemId }}</span>
            </div>
          </div>
          <div class="position-absolute z-2 d-flex flex-column justify-content-start align-items-end">
            <span v-if="tftMapSkin.rarity != 'Default'" class="rounded px-1 m-1" style="background: #0008; padding: 1px;">{{ tftMapSkin.rarity }}</span>
          </div>
        </div>
      </div>
    </div>
    <div class="d-flex justify-content-center w-100">
      <BackToTopButton/>
    </div>
  </div>
</template>