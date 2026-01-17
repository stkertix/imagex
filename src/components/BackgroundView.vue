<template>
  <div>
    <!-- ------------------------------------------------------- -->
    <!-- Background -->

    <div class="d-flex align-center mb-3">
      <v-icon icon="fi-rr-background" class="mr-2"></v-icon>
      <span class="text-h6">Background</span>
    </div>

    <v-select
      v-if="false"
      v-model="localBackground"
      :items="backgroundOptions"
      label="Pilih Background"
      variant="outlined"
      density="comfortable"
      clearable
      class="mb-3"
    ></v-select>

    <div class="d-flex flex-wrap gap-2 mb-4">
      <v-chip
        v-for="option in backgroundOptions"
        :key="option.value"
        :color="localBackground === option.value ? 'primary' : 'default'"
        :variant="localBackground === option.value ? 'flat' : 'outlined'"
        @click="toggleBackground(option.value)"
        class="cursor-pointer"
        size="small"
      >
        {{ option.title }}
      </v-chip>
    </div>
  </div>
</template>

<script>
import { backgroundOptions } from "../options/backgroundOptions.js";

export default {
  name: "BackgroundView",
  props: {
    modelValue: {
      type: String,
      default: "",
    },
  },
  emits: ["update:modelValue"],
  data() {
    return {
      backgroundOptions,
    };
  },
  computed: {
    localBackground: {
      get() {
        return this.modelValue || "";
      },
      set(value) {
        this.$emit("update:modelValue", value);
      },
    },
  },
  methods: {
    toggleBackground(value) {
      if (this.localBackground === value) {
        this.$emit("update:modelValue", "");
      } else {
        this.$emit("update:modelValue", value);
      }
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