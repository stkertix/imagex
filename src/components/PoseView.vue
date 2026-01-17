<template>
  <div>
    <!-- ------------------------------------------------------- -->
    <!-- Pose -->

    <div class="d-flex align-center mb-3">
      <v-icon icon="fi-sr-meditation" class="mr-2"></v-icon>
      <span class="text-h6">Pose</span>
    </div>

    <v-select
      v-if="false"
      :items="poseOptions"
      chips
      class="mb-3"
      clearable
      density="comfortable"
      label="Pilih Pose"
      multiple
      v-model="localPose"
      variant="outlined"
    ></v-select>

    <div class="d-flex flex-wrap  mb-4">
      <v-chip
        v-for="option in poseOptions"
        :key="option.value"
        :background-color="localPose.includes(option.value) ? getChipColor(option) : 'default'"
        :color="localPose.includes(option.value) ? getChipColor(option) : 'default'"
        :variant="localPose.includes(option.value) ? 'flat' : 'outlined'"
        @click="togglePose(option.value)"
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
import { poseOptions } from "../options/poseOptions.js";

export default {
  name: "PoseView",
  props: {
    modelValue: {
      type: Array,
      default: () => [],
    },
  },
  emits: ["update:modelValue"],
  data() {
    return {
      poseOptions,
    };
  },
  computed: {
    localPose: {
      get() {
        return this.modelValue || [];
      },
      set(value) {
        this.$emit("update:modelValue", value);
      },
    },
  },
  methods: {
    togglePose(value) {
      const currentPose = this.localPose || [];
      const index = currentPose.indexOf(value);
      let newPose;
      
      if (index > -1) {
        newPose = currentPose.filter((v) => v !== value);
      } else {
        newPose = [...currentPose, value];
      }
      
      this.$emit("update:modelValue", newPose);
    },
    getChipColor(option) {
      return option.isSpicy ? 'error' : 'primary';
    },
  },
};
</script>

