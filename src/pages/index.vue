<script lang="ts" setup>
import { onMounted } from 'vue'
import { useRocketStore } from '@/stores/rocket'
import RocketCard from '@/components/RocketCard.vue'
import LoadingState from '@/components/LoadingState.vue'
import ErrorState from '@/components/ErrorState.vue'
import EmptyState from '@/components/EmptyState.vue'

const rocketStore = useRocketStore()

onMounted(() => {
  if (!rocketStore.rockets.length) {
    rocketStore.fetchRockets()
  }
})
</script>

<template>
  <v-container class="py-8">
    <div class="mb-8">
      <p class="text-overline text-primary mb-2">
        Allo Bank Frontend Test
      </p>

      <h1 class="text-h4 text-md-h3 font-weight-bold mb-3">
        SpaceX Rocket Explorer
      </h1>

      <p class="text-body-1 text-medium-emphasis mb-0">
        Explore SpaceX rockets from Launch Library 2, view their details,
        and add temporary rockets while the app is running.
      </p>
    </div>

    <LoadingState v-if="rocketStore.loading" />

    <ErrorState
      v-else-if="rocketStore.error"
      :message="rocketStore.error"
      @retry="rocketStore.fetchRockets"
    />

    <template v-else>
      <div class="d-flex align-center justify-space-between mb-4">
        <p class="text-body-2 text-medium-emphasis mb-0">
          Showing {{ rocketStore.filteredRockets.length }} rocket(s)
        </p>
      </div>

      <v-row v-if="rocketStore.filteredRockets.length">
        <v-col
          v-for="rocket in rocketStore.filteredRockets"
          :key="rocket.id"
          cols="12"
          sm="6"
          md="4"
        >
          <RocketCard :rocket="rocket" />
        </v-col>
      </v-row>

      <EmptyState v-else />
    </template>
  </v-container>
</template>