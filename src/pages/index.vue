<template>
  <v-container class="py-8">
    <h1 class="mb-6">
      SpaceX Rockets
    </h1>

    <div v-if="rocketStore.loading">
      Loading rockets...
    </div>

    <div v-else-if="rocketStore.error">
      <p>{{ rocketStore.error }}</p>

      <v-btn
        class="mt-4"
        @click="rocketStore.fetchRockets"
      >
        Retry
      </v-btn>
    </div>

    <div v-else>
      <p class="mb-4">
        Total rockets: {{ rocketStore.rockets.length }}
      </p>

      <div
        v-for="rocket in rocketStore.rockets"
        :key="rocket.id"
        class="mb-4"
      >
        {{ rocket.full_name }}
      </div>
    </div>
  </v-container>
</template>

<script lang="ts" setup>
  import { onMounted } from 'vue'

  import { useRocketStore } from '@/stores/rocket'

  const rocketStore = useRocketStore()

  onMounted(() => {
    if (!rocketStore.rockets.length) {
      rocketStore.fetchRockets()
    }
  })
</script>