<template>
  <div class="rounded-bl-md rounded-tl-md mx-auto border-1 w-full min-w-100px relative hide-scrollbar overflow-auto grow-1">
    <OpenConfigFileInIDE
      v-slot="{onClick}"
      :gql="props.gql"
    >
      <Button
        variant="outline"
        class="top-4 right-4 absolute"
        :prefix-icon="IconCode"
        prefix-icon-class="text-gray-500"
        @click="onClick"
      >
        {{ t('file.edit') }}
      </Button>
    </OpenConfigFileInIDE>
    <code
      class="font-thin p-16px text-gray-600 text-size-14px leading-24px block"
    >
      {<br>
      <div class="pl-24px">
        <span
          v-for="{ field, value, from } in sortAlphabetical(props.gql.config)"
          :key="field"
        >
          {{ field }}:
          <RenderObject
            v-if="value && typeof value === 'object'"
            :value="value"
            :color-classes="`rounded-sm px-2px ${colorMap[from]}`"
            :from="from"
          />
          <span
            v-else
            class="rounded-sm px-2px"
            :class="colorMap[from]"
            :data-cy-config="from"
          >{{ renderPrimitive(value) }}</span>,
          <br>
        </span>
      </div>
      }
    </code>
  </div>
</template>

<script lang="ts" setup>
import Button from '@cy/components/Button.vue'
import IconCode from '~icons/mdi/code'
import { useI18n } from '@cy/i18n'
import { CONFIG_LEGEND_COLOR_MAP } from './ConfigSourceColors'
import RenderObject from './renderers/RenderObject.vue'
import { renderPrimitive } from './renderers/renderPrimitive'
import { gql } from '@urql/core'
import OpenConfigFileInIDE from '@packages/frontend-shared/src/gql-components/OpenConfigFileInIDE.vue'
import type { ConfigCodeFragment } from '../../generated/graphql'

gql`
fragment ConfigCode on CurrentProject {
  id
  config
  ...OpenConfigFileInIDE
}
`

const props = defineProps<{
  gql: ConfigCodeFragment
}>()

const sortAlphabetical = (config) => {
  return config.sort((a, b) => {
    return a.field.localeCompare(b.field)
  })
}

// a bug in vite demands that we do this passthrough
const colorMap = CONFIG_LEGEND_COLOR_MAP

const { t } = useI18n()
</script>
