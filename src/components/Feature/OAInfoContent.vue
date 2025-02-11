<script setup>
import { Badge } from '../ui/badge/index'

const props = defineProps({
  openapi: {
    type: Object,
    required: true,
  },
})
</script>

<template>
  <div class="flex flex-col">
    <div class="flex flex-col items-start">
      <div class="flex flex-row items-center gap-2">
        <Badge
          v-if="props.openapi.parsedSpec.info.version"
          variant="outline"
        >
          v{{ props.openapi.parsedSpec.info.version }}
        </Badge>
      </div>

      <OAHeading level="h1">
        {{ props.openapi.parsedSpec.info.title ?? $t('API Documentation') }}
      </OAHeading>
    </div>

    <span v-if="props.openapi.parsedSpec.info.summary" class="text-gray-600 dark:text-gray-300">
      {{ props.openapi.parsedSpec.info.summary }}
    </span>

    <OAMarkdown
      v-if="props.openapi.parsedSpec.info.description"
      :content="props.openapi.parsedSpec.info.description"
      class="mt-4"
    />

    <template v-if="props.openapi.parsedSpec.info.contact">
      <OAHeading level="h2">
        {{ $t('Contact') }}
      </OAHeading>

      <div class="flex flex-row items-center gap-2">
        <template v-if="props.openapi.parsedSpec.info.contact.url">
          <a :href="props.openapi.parsedSpec.info.contact.url" :aria-label="props.openapi.parsedSpec.info.contact.name ?? $t('Contact')">
            {{ props.openapi.parsedSpec.info.contact.name ?? $t('Contact') }}
          </a>

          <span v-if="props.openapi.parsedSpec.info.contact.email" class="text-gray-400 dark:text-gray-500">/</span>
        </template>

        <a v-if="props.openapi.parsedSpec.info.contact.email" :href="`mailto:${props.openapi.parsedSpec.info.contact.email}`" :aria-label="props.openapi.parsedSpec.info.contact.email">
          {{ props.openapi.parsedSpec.info.contact.email }}
        </a>
      </div>
    </template>

    <div class="grid grid-cols-1 sm:grid-cols-2 gap-2">
      <div v-if="props.openapi.parsedSpec.info.termsOfService">
        <OAHeading level="h2">
          {{ $t('Terms of Service') }}
        </OAHeading>

        <a :href="props.openapi.parsedSpec.info.termsOfService" :aria-label="props.openapi.parsedSpec.info.termsOfService">
          {{ props.openapi.parsedSpec.info.termsOfService }}
        </a>
      </div>

      <div v-if="props.openapi.parsedSpec.info.license">
        <OAHeading level="h2">
          {{ $t('License') }}
        </OAHeading>

        <a :href="props.openapi.parsedSpec.info.license.url" :aria-label="props.openapi.parsedSpec.info.license.name">
          {{ props.openapi.parsedSpec.info.license.name ?? $t('License') }}
        </a>
      </div>
    </div>

    <template v-if="Object.keys(props.openapi.parsedSpec.externalDocs).length">
      <OAHeading level="h2">
        {{ $t('External Documentation') }}
      </OAHeading>

      <a :href="props.openapi.parsedSpec.externalDocs.url" :aria-label="props.openapi.parsedSpec.externalDocs.description ?? $t('External Documentation')">
        {{ props.openapi.parsedSpec.externalDocs.description ?? $t('External Documentation') }}
      </a>
    </template>
  </div>
</template>
