<script setup>
import { inject } from 'vue'
import { OPENAPI_GLOBAL_KEY, OPENAPI_LOCAL_KEY } from '../../composables/useOpenapi'
import { getOpenApiInstance } from '../../lib/getOpenApiInstance'
import OAContext from './OAContext.vue'

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

// const globalOpenApi = inject(OPENAPI_GLOBAL_KEY, undefined)

// const localOpenApi = inject(OPENAPI_LOCAL_KEY, undefined)

// const openapi = props.openapi ?? getOpenApiInstance({
//   custom: { spec: props.spec },
//   injected: globalOpenApi,
//   injectedLocal: localOpenApi,
// })
</script>

<template>
  <Suspense>
    <OAContext :spec="spec">
      <template #default="{ openapi: openapiContext }">
        <slot :openapi="openapiContext" />
      </template>
    </OAContext>
  </Suspense>
</template>
