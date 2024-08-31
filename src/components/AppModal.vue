<script setup>
import { defineProps, defineEmits } from 'vue'
import { TransitionRoot, TransitionChild, Dialog, DialogPanel, DialogTitle } from '@headlessui/vue'

defineProps({
  open: {
    type: Boolean,
    default: false
  },
  asPage: {
    type: Boolean,
    default: false
  },
  title: String,
  icon: {
    type: String,
    default: 'warning'
  },
  width: {
    type: String,
    default: 'max-w-md'
  },
  padding: {
    type: String,
    default: 'p-6'
  },
  actions: {
    type: Boolean,
    default: true
  },
  cancelable: {
    type: Boolean,
    default: true
  },
  cancelLabel: {
    type: String
  },
  confirmable: {
    type: Boolean,
    default: true
  },
  confirmLabel: {
    type: String
  },
  style: {
    type: String
  }
})

const emit = defineEmits(['close', 'confirm'])

function closeModal() {
  emit('close')
}
</script>

<template>
  <TransitionRoot appear :show="open" as="template">
    <Dialog as="div" class="relative z-[999999]" @close="closeModal">
      <TransitionChild
        as="template"
        enter="duration-300 ease-out"
        enter-from="opacity-0"
        enter-to="opacity-100"
        leave="duration-200 ease-in"
        leave-from="opacity-100"
        leave-to="opacity-0"
      >
        <div class="fixed inset-0 bg-brand-600/25 backdrop-blur-sm" />
      </TransitionChild>

      <div class="fixed inset-0 overflow-y-auto">
        <div class="flex min-h-full items-start justify-center p-4 py-[10vh] text-center">
          <TransitionChild
            as="template"
            enter="duration-300 ease-out"
            enter-from="opacity-0 scale-75 -translate-y-10"
            enter-to="opacity-100 scale-100"
            leave="duration-200 ease-in"
            leave-from="opacity-100 scale-100"
            leave-to="opacity-0 scale-75 -translate-y-10"
          >
            <DialogPanel
              :class="[width, padding]"
              class="relative mt-[10%] w-full overflow-hidden rounded-2xl bg-white align-middle shadow-soft-lg transition-all text-start dark:bg-dark-2 dark:ring-2 dark:ring-stone-700"
            >
              <icon-tabler-x
                class="absolute top-2 z-50 h-8 w-8 cursor-pointer rounded-full p-1.5 text-gray-900 text-primary end-2 end-2 float-end dark:bg-dark-4"
                @click="closeModal"
              />

              <div v-if="title" class="mb-4 flex items-center pt-4 space-s-3">
                <DialogTitle
                  as="h3"
                  class="flex-1 text-lg font-bold leading-6 text-brand-900 dark:text-brand-50"
                >
                  {{ title }}
                </DialogTitle>
                <slot name="title" />
              </div>
              <div :class="{ 'mt-2': !!title }">
                <slot />
              </div>

              <div v-if="actions" class="-m-2 mt-6 flex justify-end space-s-3">
                <button
                  v-if="cancelable"
                  type="button"
                  class="inline-flex justify-center rounded-md border border-transparent bg-gray-50 px-4 py-2 text-sm font-medium text-gray-900 hover:bg-blue-100 dark:bg-dark-3 dark:text-brand-50"
                  @click="closeModal"
                >
                  {{ cancelLabel }}
                </button>
                <button
                  v-if="confirmable"
                  type="button"
                  class="inline-flex justify-center rounded-md border border-transparent bg-brand-800 px-4 py-2 text-sm font-medium text-white hover:bg-brand-900 dark:bg-primary"
                  @click="emit('confirm')"
                >
                  {{ confirmLabel }}
                </button>
              </div>
            </DialogPanel>
          </TransitionChild>
        </div>
      </div>
    </Dialog>
  </TransitionRoot>
</template>
