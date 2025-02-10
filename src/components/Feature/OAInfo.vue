<script setup>
import { inject } from 'vue'
import { OPENAPI_GLOBAL_KEY, OPENAPI_LOCAL_KEY } from '../../composables/useOpenapi'
import { getOpenApiInstance } from '../../lib/getOpenApiInstance'
import { Badge } from '../ui/badge/index'
import OAContext from './OAContext.vue'

const props = defineProps({
  spec: {
    type: Object,
    required: false,
  },
  openapi: {
    type: Object,
    required: false,
  },
})

const openapi = props.openapi ?? getOpenApiInstance({
  custom: { spec: props.spec },
  injected: inject(OPENAPI_GLOBAL_KEY, undefined),
  injectedLocal: inject(OPENAPI_LOCAL_KEY, undefined),
})
</script>

<template>
  <OAContext :openapi="openapi" :spec="spec">
    <template #default="{ openapi }">
      <div class="flex flex-col">
        <div class="flex flex-col items-start">
          <div class="flex flex-row items-center gap-2">
            <Badge
              v-if="openapi.parsedSpec.info.version"
              variant="outline"
            >
              v{{ openapi.parsedSpec.info.version }}
            </Badge>
          </div>

          <OAHeading level="h1">
            {{ openapi.parsedSpec.info.title ?? $t('API Documentation') }}
          </OAHeading>
        </div>

        <span v-if="openapi.parsedSpec.info.summary" class="text-gray-600 dark:text-gray-300">
          {{ openapi.parsedSpec.info.summary }}
        </span>

        <OAMarkdown
          v-if="openapi.parsedSpec.info.description"
          :content="openapi.parsedSpec.info.description"
          class="mt-4"
        />

        <template v-if="openapi.parsedSpec.info.contact">
          <OAHeading level="h2">
            {{ $t('Contact') }}
          </OAHeading>

          <div class="flex flex-row items-center gap-2">
            <template v-if="openapi.parsedSpec.info.contact.url">
              <a :href="openapi.parsedSpec.info.contact.url" :aria-label="openapi.parsedSpec.info.contact.name ?? $t('Contact')">
                {{ openapi.parsedSpec.info.contact.name ?? $t('Contact') }}
              </a>

              <span v-if="openapi.parsedSpec.info.contact.email" class="text-gray-400 dark:text-gray-500">/</span>
            </template>

            <a v-if="openapi.parsedSpec.info.contact.email" :href="`mailto:${openapi.parsedSpec.info.contact.email}`" :aria-label="openapi.parsedSpec.info.contact.email">
              {{ openapi.parsedSpec.info.contact.email }}
            </a>
          </div>
        </template>

        <div class="grid grid-cols-1 sm:grid-cols-2 gap-2">
          <div v-if="openapi.parsedSpec.info.termsOfService">
            <OAHeading level="h2">
              {{ $t('Terms of Service') }}
            </OAHeading>

            <a :href="openapi.parsedSpec.info.termsOfService" :aria-label="openapi.parsedSpec.info.termsOfService">
              {{ openapi.parsedSpec.info.termsOfService }}
            </a>
          </div>

          <div v-if="openapi.parsedSpec.info.license">
            <OAHeading level="h2">
              {{ $t('License') }}
            </OAHeading>

            <a :href="openapi.parsedSpec.info.license.url" :aria-label="openapi.parsedSpec.info.license.name">
              {{ openapi.parsedSpec.info.license.name ?? $t('License') }}
            </a>
          </div>
        </div>

        <template v-if="Object.keys(openapi.parsedSpec.externalDocs).length">
          <OAHeading level="h2">
            {{ $t('External Documentation') }}
          </OAHeading>

          <a :href="openapi.parsedSpec.externalDocs.url" :aria-label="openapi.parsedSpec.externalDocs.description ?? $t('External Documentation')">
            {{ openapi.parsedSpec.externalDocs.description ?? $t('External Documentation') }}
          </a>
        </template>
      </div>
    </template>
  </OAContext>
</template>
