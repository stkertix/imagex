<template>
  <div>
    <!-- ------------------------------------------------------- -->
    <!-- Camera Shot -->

    <div class="d-flex align-center mb-3">
      <v-icon icon="fi-rr-camera" class="mr-2"></v-icon>
      <span class="text-h6">Camera Shot</span>
    </div>

    <v-select
      v-if="false"
      v-model="localCameraShot"
      :items="cameraShotOptions"
      label="Pilih Camera Shot"
      variant="outlined"
      density="comfortable"
      clearable
      class="mb-3"
    ></v-select>

    <div class="d-flex flex-wrap gap-2 mb-4">
      <v-chip
        v-for="option in cameraShotOptions"
        :key="option.value"
        :color="localCameraShot === option.value ? 'primary' : 'default'"
        :variant="localCameraShot === option.value ? 'flat' : 'outlined'"
        @click="toggleCameraShot(option.value)"
        class="cursor-pointer"
        size="small"
      >
        {{ option.title }}
      </v-chip>
    </div>
  </div>
</template>

<script>
import { cameraShotOptions } from "../options/cameraShotOptions.js";

export default {
  name: "CameraShotView",
  props: {
    modelValue: {
      type: String,
      default: "",
    },
  },
  emits: ["update:modelValue"],
  data() {
    return {
      cameraShotOptions,
    };
  },
  computed: {
    localCameraShot: {
      get() {
        return this.modelValue || "";
      },
      set(value) {
        this.$emit("update:modelValue", value);
      },
    },
  },
  methods: {
    toggleCameraShot(value) {
      if (this.localCameraShot === value) {
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