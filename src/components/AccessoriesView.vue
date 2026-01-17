<template>
  <div>
    <!-- ------------------------------------------------------- -->
    <!-- Accessories -->

    <div class="d-flex align-center mb-3">
      <v-icon icon="fi-rr-sunglasses" class="mr-2"></v-icon>
      <span class="text-h6">Accessories</span>
    </div>

    <v-select
      v-if="false"
      v-model="localAccessories"
      :items="accessoriesOptions"
      label="Pilih Accessories"
      variant="outlined"
      density="comfortable"
      multiple
      chips
      clearable
      class="mb-3"
    ></v-select>

    <div class="d-flex flex-wrap gap-2 mb-4">
      <v-chip
        v-for="option in accessoriesOptions"
        :key="option.value"
        :color="localAccessories.includes(option.value) ? 'primary' : 'default'"
        :variant="localAccessories.includes(option.value) ? 'flat' : 'outlined'"
        @click="toggleAccessories(option.value)"
        class="cursor-pointer"
        size="small"
      >
        {{ option.title }}
      </v-chip>
    </div>
  </div>
</template>

<script>
import { accessoriesOptions } from "../options/accessoriesOptions.js";

export default {
  name: "AccessoriesView",
  props: {
    modelValue: {
      type: Array,
      default: () => [],
    },
  },
  emits: ["update:modelValue"],
  data() {
    return {
      accessoriesOptions,
    };
  },
  computed: {
    localAccessories: {
      get() {
        return this.modelValue || [];
      },
      set(value) {
        this.$emit("update:modelValue", value);
      },
    },
  },
  methods: {
    toggleAccessories(value) {
      const currentAccessories = this.localAccessories || [];
      const index = currentAccessories.indexOf(value);
      let newAccessories;
      
      if (index > -1) {
        newAccessories = currentAccessories.filter((v) => v !== value);
      } else {
        newAccessories = [...currentAccessories, value];
      }
      
      this.$emit("update:modelValue", newAccessories);
    },
  },
};
</script>

<style scoped>
.cursor-pointer {
  cursor: pointer;
  transition: all 0.2s ease-in-out;
}

.gap-2 {
  gap: 8px;
}
</style>