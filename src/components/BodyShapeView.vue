<template>
  <div>
    <!-- ------------------------------------------------------- -->
    <!-- Body Shape -->

    <div class="d-flex align-center mb-3">
      <v-icon icon="fi-sr-person-simple" class="mr-2"></v-icon>
      <span class="text-h6">Body Shape</span>
    </div>

    <v-select
      v-if="false"
      v-model="localBodyShape"
      :items="bodyShapeOptions"
      label="Pilih Body Shape"
      variant="outlined"
      density="comfortable"
      multiple
      chips
      clearable
      class="mb-3"
    ></v-select>

    <div class="d-flex flex-wrap gap-2 mb-4">
      <v-chip
        v-for="option in bodyShapeOptions"
        :key="option.value"
        :color="localBodyShape.includes(option.value) ? 'primary' : 'default'"
        :variant="localBodyShape.includes(option.value) ? 'flat' : 'outlined'"
        @click="toggleBodyShape(option.value)"
        class="cursor-pointer"
        size="small"
      >
        {{ option.title }}
      </v-chip>
    </div>
  </div>
</template>

<script>
import { bodyShapeOptions } from "../options/bodyShapeOptions.js";

export default {
  name: "BodyShapeView",
  props: {
    modelValue: {
      type: Array,
      default: () => [],
    },
  },
  emits: ["update:modelValue"],
  data() {
    return {
      bodyShapeOptions,
    };
  },
  computed: {
    localBodyShape: {
      get() {
        return this.modelValue || [];
      },
      set(value) {
        this.$emit("update:modelValue", value);
      },
    },
  },
  methods: {
    toggleBodyShape(value) {
      const currentBodyShape = this.localBodyShape || [];
      const index = currentBodyShape.indexOf(value);
      let newBodyShape;
      
      if (index > -1) {
        newBodyShape = currentBodyShape.filter((v) => v !== value);
      } else {
        newBodyShape = [...currentBodyShape, value];
      }
      
      this.$emit("update:modelValue", newBodyShape);
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