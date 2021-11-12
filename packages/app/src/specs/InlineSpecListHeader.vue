<template>
  <div class="h-64px items-center gap-8px mx-16px border-b-1 border-gray-900 grid grid-cols-[minmax(0,1fr),63px,24px]">
    <div class="relative items-center group">
      <div class="absolute inset-y-0 flex items-center pointer-events-none">
        <i-cy-magnifying-glass_x16
          :class="inputFocused ? 'icon-dark-gray-200' : 'icon-dark-gray-900'"
          class="icon-light-gray-1000"
        />
      </div>
      <input
        class="
          w-full
          bg-gray-1000
          pl-6
          text-gray-500
          placeholder-gray-700
          font-light
          outline-none
        "
        placeholder="Search Specs"
        :value="search"
        @focus="inputFocused = true"
        @blur="inputFocused = false"
        @input="onInput"
      >
    </div>
    <RadioGroup
      :model-value="tab"
      class="flex border-1 border-gray-900 rounded-md h-24px w-64px text-md cursor-pointer"
      @update:model-value="emit('update:tab', $event)"
    >
      <RadioGroupOption
        v-slot="{ checked }"
        as="template"
        value="file-list"
        data-cy="file-list-radio-option"
      >
        <div
          class="flex flex-1 items-center justify-center outline-none"
          :class="{ 'bg-gray-900': checked }"
        >
          <i-cy-file-list
            :class="checked ? 'icon-dark-gray-200' : 'icon-dark-gray-700'"
          />
        </div>
      </RadioGroupOption>
      <RadioGroupOption
        v-slot="{ checked }"
        as="template"
        value="file-tree"
        data-cy="file-tree-radio-option"
      >
        <div
          class="flex flex-1 items-center justify-center outline-none"
          :class="{ 'bg-gray-900': checked }"
        >
          <i-cy-file-tree
            :class="checked ? 'icon-dark-gray-200' : 'icon-dark-gray-700'"
          />
        </div>
      </RadioGroupOption>
    </RadioGroup>
    <button
      class="
        border-1 border-gray-900
        h-24px
        w-24px
        rounded-md
        add-button
        outline-none
        flex
        items-center
        justify-center
      "
      data-cy="runner-spec-list-add-spec"
      @click="emit('addSpec')"
    >
      <i-cy-add-small_x16 class="icon-light-gray-50 icon-dark-gray-200" />
    </button>
  </div>
</template>

<script lang="ts" setup>
import Input from '@cy/components/Input.vue'
import Button from '@cy/components/Button.vue'
import { ref } from 'vue'
import { RadioGroup, RadioGroupOption } from '@headlessui/vue'

defineProps<{tab: string, search: string}>()

const emit = defineEmits<{
  (e: 'update:tab', tab: string): void
  (e: 'update:search', search: string): void
  (e: 'addSpec'): void
}>()

const inputFocused = ref(false)

const onInput = (e: Event) => {
  const value = (e.target as HTMLInputElement).value

  emit('update:search', value)
}

</script>

<style>
/** Windi box shadows are dark, so styles are for lighter box shadows */
.add-button {
  transition: box-shadow 0.3s ease-in-out;
}

.add-button:hover {
  box-shadow: 0 0 4px rgba(191, 194, 212, 0.6);
}
</style>