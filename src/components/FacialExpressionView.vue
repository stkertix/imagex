<template>
  <div>
    <!-- ------------------------------------------------------- -->
    <!-- Facial Expression -->

    <div class="d-flex align-center mb-3">
      <v-icon icon="fi-rr-face-expressionless" class="mr-2"></v-icon>
      <span class="text-h6">Facial Expression</span>
    </div>

    <v-select
      v-if="false"
      v-model="localFacialExpression"
      :items="facialExpressionOptions"
      label="Pilih Facial Expression"
      variant="outlined"
      density="comfortable"
      multiple
      chips
      clearable
      class="mb-3"
    ></v-select>

    <div class="d-flex flex-wrap  mb-4">
      <v-chip
        v-for="option in facialExpressionOptions"
        :key="option.value"
        :background-color="localFacialExpression.includes(option.value) ? getChipColor(option) : 'default'"
        :color="localFacialExpression.includes(option.value) ? getChipColor(option) : 'default'"
        :variant="localFacialExpression.includes(option.value) ? 'flat' : 'outlined'"
        @click="toggleFacialExpression(option.value)"
        class="ma-1"
        size="small"
      >
        {{ option.title }}

        <v-icon
          v-if="option.isSpicy"
          icon="fi-sr-flame"
          class="ms-1 text-error"
        ></v-icon>
      </v-chip>
    </div>
  </div>
</template>

<script>
import { facialExpressionOptions } from "../options/facialExpressionOptions.js";

export default {
  name: "FacialExpressionView",
  props: {
    modelValue: {
      type: Array,
      default: () => [],
    },
  },
  emits: ["update:modelValue"],
  data() {
    return {
      facialExpressionOptions,
    };
  },
  computed: {
    localFacialExpression: {
      get() {
        return this.modelValue || [];
      },
      set(value) {
        this.$emit("update:modelValue", value);
      },
    },
  },
  methods: {
    toggleFacialExpression(value) {
      const currentFacialExpression = this.localFacialExpression || [];
      const index = currentFacialExpression.indexOf(value);
      let newFacialExpression;
      
      if (index > -1) {
        newFacialExpression = currentFacialExpression.filter((v) => v !== value);
      } else {
        newFacialExpression = [...currentFacialExpression, value];
      }
      
      this.$emit("update:modelValue", newFacialExpression);
    },
    getChipColor(option) {
      return option.isSpicy ? 'error' : 'primary';
    },
  },
};
</script>

