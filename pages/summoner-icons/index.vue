<script setup lang="ts">
const { client } = useClient();

const { data: icons } = await useLocalizedData(async (x) => (await client.summonerIcons.listAsync({locale: x, version: "latest"}))
.sort((a, b) => a.id - b.id));

const { query, paginate } = useQueryable(icons, (x) => x.id, (x) => x.title);
const pagination = paginate(100);
</script>

<template>
  <div class="d-flex flex-column gap-4">
    <div class="d-flex flex-wrap justify-content-end gap-2">
        <Card class="d-flex justify-content-center align-items-center me-auto">
            <span>{{ icons.length }} summoner icons</span>
        </Card>
        <Pagination :pagination="pagination"/>
        <Search v-model="query"/>
    </div>

    <div class="d-flex flex-wrap justify-content-center gap-4">
        <div style="width: 200px;" v-for="summonerIcon in pagination.pages[pagination.index.value]" :key="summonerIcon.id"
            data-aos="zoom-out"
            data-aos-duration="500">
            <div class="ratio ratio-1x1 position-relative trans-hover-grow">
                <LLazyImg :src="summonerIcon.getImage('latest')" class="rounded" img-class="rounded"/>
                <div class="position-absolute z-1 d-flex flex-column justify-content-end">
                    <div class="d-inline-flex justify-content-between align-items-end bg-dark-gradient rounded-bottom p-2">
                        <span>{{ summonerIcon.title }}</span>
                        <span class="fw-bold">{{ summonerIcon.id }}</span>
                    </div>
                </div>
                <div class="position-absolute z-2 d-flex flex-column justify-content-start align-items-end">
                    <div class="d-inline-flex justify-content-end gap-1 m-1">
                        <span style="background: #0008; padding: 1px;" class="px-1 rounded">{{ summonerIcon.yearReleased }}</span>
                        <span v-if="summonerIcon.isLegacy" class="px-1 rounded" style="background: #0008; padding: 1px;">Legacy</span>
                        <span v-if="summonerIcon.rarities && summonerIcon.rarities.length > 0 && summonerIcon.rarities[0].rarity != '0'" class="px-1 rounded" style="background: #0008; padding: 1px;">{{ useRarities(summonerIcon.rarities[0].rarity) }}</span>
                    </div>
                </div>
            </div>
        </div>
    </div>

    <div class="d-flex justify-content-center w-100">
        <BackToTopButton/>
    </div>
  </div>
</template>