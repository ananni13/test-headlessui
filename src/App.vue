<template>
  <div class="pt-32 grid grid-cols-3 gap-4 justify-items-center">
    <div>
      <Switch
        v-model="enabled"
        :class="enabled ? 'bg-light-blue-700' : 'bg-light-blue-300'"
        class="relative inline-flex flex-shrink-0 h-[38px] w-[74px] border-2 border-transparent rounded-full cursor-pointer transition-colors ease-in-out duration-200 focus:outline-none focus-visible:ring-2 focus-visible:ring-white focus-visible:ring-opacity-75"
      >
        <span class="sr-only">Use setting</span>
        <span
          aria-hidden="true"
          :class="enabled ? 'translate-x-9' : 'translate-x-0'"
          class="pointer-events-none inline-block h-[34px] w-[34px] rounded-full bg-white shadow-lg transform ring-0 transition ease-in-out duration-200"
        />
      </Switch>
    </div>

    <div class="w-72">
      <Listbox v-model="selectedPerson">
        <div class="relative mt-1">
          <ListboxButton
            class="relative w-full py-2 pl-3 pr-10 text-left bg-white rounded-lg shadow-md cursor-default focus:outline-none focus-visible:ring-2 focus-visible:ring-opacity-75 focus-visible:ring-white focus-visible:ring-offset-orange-300 focus-visible:ring-offset-2 focus-visible:border-indigo-500 sm:text-sm"
          >
            <span class="block truncate">{{ selectedPerson.name }}</span>
            <span
              class="absolute inset-y-0 right-0 flex items-center pr-2 pointer-events-none"
            >
              <SelectorIcon class="w-5 h-5 text-gray-400" aria-hidden="true" />
            </span>
          </ListboxButton>

          <transition
            leave-active-class="transition duration-100 ease-in"
            leave-from-class="opacity-100"
            leave-to-class="opacity-0"
          >
            <ListboxOptions
              class="absolute w-full py-1 mt-1 overflow-auto text-base bg-white rounded-md shadow-lg max-h-60 ring-1 ring-black ring-opacity-5 focus:outline-none sm:text-sm"
            >
              <ListboxOption
                v-slot="{ active, selected }"
                v-for="person in people"
                :key="person"
                :value="person"
                as="template"
              >
                <li
                  :class="[
                    active ? 'text-blue-900 bg-blue-100' : 'text-gray-900',
                    'cursor-default select-none relative py-2 pl-10 pr-4',
                  ]"
                >
                  <span
                    :class="[
                      selected ? 'font-medium' : 'font-normal',
                      'block truncate',
                    ]"
                    >{{ person.name }}</span
                  >
                  <span
                    v-if="selected"
                    class="absolute inset-y-0 left-0 flex items-center pl-3 text-blue-600"
                  >
                    <CheckIcon class="w-5 h-5" aria-hidden="true" />
                  </span>
                </li>
              </ListboxOption>
            </ListboxOptions>
          </transition>
        </div>
      </Listbox>
    </div>

    <div class="w-full max-w-md mx-auto">
      <RadioGroup v-model="selected">
        <RadioGroupLabel class="sr-only">Server size</RadioGroupLabel>
        <div class="space-y-2">
          <RadioGroupOption
            as="template"
            v-for="plan in plans"
            :key="plan.name"
            :value="plan"
            v-slot="{ active, checked }"
          >
            <div
              :class="[
                active
                  ? 'ring-2 ring-offset-2 ring-offset-light-blue-300 ring-white ring-opacity-60'
                  : '',
                checked
                  ? 'bg-light-blue-900 bg-opacity-75 text-white '
                  : 'bg-white ',
              ]"
              class="relative flex px-5 py-4 rounded-lg shadow-md cursor-pointer focus:outline-none"
            >
              <div class="flex items-center justify-between w-full">
                <div class="flex items-center">
                  <div class="text-sm">
                    <RadioGroupLabel
                      as="p"
                      :class="checked ? 'text-white' : 'text-gray-900'"
                      class="font-medium"
                    >
                      {{ plan.name }}
                    </RadioGroupLabel>
                    <RadioGroupDescription
                      as="span"
                      :class="checked ? 'text-light-blue-100' : 'text-gray-500'"
                      class="inline"
                    >
                      <span> {{ plan.ram }}/{{ plan.cpus }}</span>
                      <span aria-hidden="true"> &middot; </span>
                      <span>{{ plan.disk }}</span>
                    </RadioGroupDescription>
                  </div>
                </div>
                <div v-show="checked" class="flex-shrink-0 text-white">
                  <svg class="w-6 h-6" viewBox="0 0 24 24" fill="none">
                    <circle
                      cx="12"
                      cy="12"
                      r="12"
                      fill="#fff"
                      fill-opacity="0.2"
                    />
                    <path
                      d="M7 13l3 3 7-7"
                      stroke="#fff"
                      stroke-width="1.5"
                      stroke-linecap="round"
                      stroke-linejoin="round"
                    />
                  </svg>
                </div>
              </div>
            </div>
          </RadioGroupOption>
        </div>
      </RadioGroup>
    </div>

  </div>
</template>

<script lang="ts">
import { defineComponent, ref } from 'vue'
import {
  Listbox,
  ListboxLabel,
  ListboxButton,
  ListboxOptions,
  ListboxOption,
  RadioGroup,
  RadioGroupLabel,
  RadioGroupDescription,
  RadioGroupOption,
  Switch
} from '@headlessui/vue'
import { CheckIcon, SelectorIcon } from '@heroicons/vue/solid'

export default defineComponent({
  name: 'App',
  components: {
    CheckIcon,
    SelectorIcon,
    Listbox,
    ListboxLabel,
    ListboxButton,
    ListboxOptions,
    ListboxOption,
    RadioGroup,
    RadioGroupLabel,
    RadioGroupDescription,
    RadioGroupOption,
    Switch
  },
  setup() {
    const enabled = ref(false)

    const people = [
      { name: "Wade Cooper" },
      { name: "Arlene Mccoy" },
      { name: "Devon Webb" },
      { name: "Tom Cook" },
      { name: "Tanya Fox" },
      { name: "Hellen Schmidt" },
    ];
    const selectedPerson = ref(people[0]);

    const plans = [
      {
        name: "Startup",
        ram: "12GB",
        cpus: "6 CPUs",
        disk: "160 GB SSD disk",
      },
      {
        name: "Business",
        ram: "16GB",
        cpus: "8 CPUs",
        disk: "512 GB SSD disk",
      },
      {
        name: "Enterprise",
        ram: "32GB",
        cpus: "12 CPUs",
        disk: "1024 GB SSD disk",
      },
    ];
    const selected = ref(plans[0]);

    return {
      enabled,
      people,
      selectedPerson,
      plans,
      selected
    }
  }
})
</script>
