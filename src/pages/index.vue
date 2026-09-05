<template>
  <!-- <v-container class="py-8">
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
  </v-container> -->

  <v-container class="py-8">
    <div class="mb-8">
      <h1 class="text-h4 font-weight-bold">
        SpaceX Rockets
      </h1>

      <p class="text-medium-emphasis mt-2">
        Explore SpaceX rocket configurations.
      </p>
    </div>

    <div v-if="rocketStore.loading">
      <v-row>
        <v-col
          v-for="index in 6"
          :key="index"
          cols="12"
          sm="6"
          md="4"
        >
          <v-skeleton-loader
            type="image, heading, paragraph"
          />
        </v-col>
      </v-row>
    </div>

    <div v-else-if="rocketStore.error">
      <v-alert
        type="error"
        variant="tonal"
      >
        {{ rocketStore.error }}
      </v-alert>

      <v-btn
        class="mt-4"
        color="primary"
        @click="rocketStore.fetchRockets"
      >
        Retry
      </v-btn>
    </div>

    <div v-else>
      <p class="mb-4 text-medium-emphasis">
        {{ rocketStore.rockets.length }} rockets found
      </p>

      <v-row>
        <v-col
          v-for="rocket in rocketStore.rockets"
          :key="rocket.id"
          cols="12"
          sm="6"
          md="4"
        >
          <RocketCard :rocket="rocket" />
        </v-col>
      </v-row>
    </div>
  </v-container>
</template>

<script lang="ts" setup>
  import { onMounted } from 'vue'

  import RocketCard from '@/components/RocketCard.vue'
  import { useRocketStore } from '@/stores/rocket'

  const rocketStore = useRocketStore()

  onMounted(() => {
    if (!rocketStore.rockets.length) {
      rocketStore.fetchRockets()
    }
  })
</script>