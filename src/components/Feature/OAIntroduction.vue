<script setup>
import { inject } from 'vue'
import { OPENAPI_GLOBAL_KEY, OPENAPI_LOCAL_KEY } from '../../composables/useOpenapi'
import { getOpenApiInstance } from '../../lib/getOpenApiInstance'
import OAContext from './OAContext.vue'
import OAInfoContent from './OAInfoContent.vue'
import OAServersContent from './OAServersContent.vue'

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
    <template #default="{ openapi: openapiContext }">
      <OAInfoContent :openapi="openapiContext" />
      <OAServersContent :openapi="openapiContext" />
    </template>
  </OAContext>
</template>
