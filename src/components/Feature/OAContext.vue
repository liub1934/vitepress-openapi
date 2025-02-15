<script setup>
import { inject, provide } from 'vue'
import { OPENAPI_GLOBAL_KEY, OPENAPI_LOCAL_KEY } from '../../composables/useOpenapi'
import { getOpenApiInstance } from '../../lib/getOpenApiInstance'

const props = defineProps({
  spec: {
    type: Object,
    required: false,
    default: null,
  },
  openapi: {
    type: Object,
    required: false,
    default: null,
  },
})

const globalOpenApi = inject(OPENAPI_GLOBAL_KEY, undefined)

const openapi
    = await getOpenApiInstance({
      custom: { spec: props.spec },
      injected: globalOpenApi,
    })

provide(OPENAPI_LOCAL_KEY, openapi)
</script>

<template>
  <div>
    <slot :openapi="openapi" />
  </div>
</template>
