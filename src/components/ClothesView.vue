<template>
  <div>
    <!-- ------------------------------------------------------- -->
    <!-- Clothes -->

    <div class="d-flex align-center mb-3">
      <v-icon icon="fi-rr-tshirt" class="mr-2"></v-icon>
      <span class="text-h6">Clothes</span>
    </div>

    <!-- Atasan -->
    <div class="mb-3">
      <div class="d-flex align-center mb-2">
        <span>Top Clothes</span>
      </div>

      <v-select
        v-if="false"
        v-model="localTopClothes"
        :items="clothesTopOptions"
        label="Atasan"
        variant="outlined"
        density="comfortable"
        clearable
        class="mb-2"
      ></v-select>

      <div class="d-flex flex-wrap gap-2 mb-2">
        <v-chip
          v-for="option in clothesTopOptions"
          :key="option.value"
          :color="localTopClothes === option.value ? 'primary' : 'default'"
          :variant="localTopClothes === option.value ? 'flat' : 'outlined'"
          @click="toggleTopClothes(option.value)"
          class="cursor-pointer"
          size="small"
        >
          {{ option.title }}
        </v-chip>
      </div>

      <!-- Top Clothes Color -->
      <div class="d-flex align-center mb-2">
        <span class="text-caption">Top Clothes Color:</span>
      </div>
      <div class="d-flex flex-wrap gap-2">
        <v-chip
          v-for="color in colorOptions"
          :key="color.value"
          :color="color.value"
          variant="elevated"
          @click="toggleTopClothesColor(color.value)"
          class="cursor-pointer"
          size="small"
        >
          {{ localTopClothesColor === color.value ? color.title : "" }}
        </v-chip>
      </div>
    </div>

    <!-- Bottom Clothes -->
    <div class="mb-4">
      <div class="d-flex align-center mb-2">
        <span>Bottom Clothes</span>
      </div>

      <v-select
        v-if="false"
        v-model="localBottomClothes"
        :items="clothesBottomOptions"
        label="Bottom Clothes"
        variant="outlined"
        density="comfortable"
        clearable
        class="mb-2"
      ></v-select>

      <div class="d-flex flex-wrap gap-2 mb-2">
        <v-chip
          v-for="option in clothesBottomOptions"
          :key="option.value"
          :color="localBottomClothes === option.value ? 'primary' : 'default'"
          :variant="localBottomClothes === option.value ? 'flat' : 'outlined'"
          @click="toggleBottomClothes(option.value)"
          class="cursor-pointer"
          size="small"
        >
          {{ option.title }}
        </v-chip>
      </div>

      <!-- Bottom Clothes Color -->
      <div class="d-flex align-center mb-2">
        <span class="text-caption">Bottom Clothes Color:</span>
      </div>
      <div class="d-flex flex-wrap gap-2">
        <v-chip
          v-for="color in colorOptions"
          :key="color.value"
          :color="color.value"
          variant="elevated"
          @click="toggleBottomClothesColor(color.value)"
          class="cursor-pointer"
          size="small"
        >
          {{ localBottomClothesColor === color.value ? color.title : "" }}
        </v-chip>
      </div>
    </div>
  </div>
</template>

<script>
import { clothesTopOptions } from "../options/clothesTopOptions.js";
import { clothesBottomOptions } from "../options/clothesBottomOptions.js";
import { colorOptions } from "../options/colorOptions.js";

export default {
  name: "ClothesView",
  props: {
    topClothes: {
      type: String,
      default: "",
    },
    topClothesColor: {
      type: String,
      default: "",
    },
    bottomClothes: {
      type: String,
      default: "",
    },
    bottomClothesColor: {
      type: String,
      default: "",
    },
  },
  emits: ["update:topClothes", "update:topClothesColor", "update:bottomClothes", "update:bottomClothesColor"],
  data() {
    return {
      clothesTopOptions,
      clothesBottomOptions,
      colorOptions,
    };
  },
  computed: {
    localTopClothes: {
      get() {
        return this.topClothes || "";
      },
      set(value) {
        this.$emit("update:topClothes", value);
      },
    },
    localTopClothesColor: {
      get() {
        return this.topClothesColor || "";
      },
      set(value) {
        this.$emit("update:topClothesColor", value);
      },
    },
    localBottomClothes: {
      get() {
        return this.bottomClothes || "";
      },
      set(value) {
        this.$emit("update:bottomClothes", value);
      },
    },
    localBottomClothesColor: {
      get() {
        return this.bottomClothesColor || "";
      },
      set(value) {
        this.$emit("update:bottomClothesColor", value);
      },
    },
  },
  methods: {
    toggleTopClothes(value) {
      if (this.localTopClothes === value) {
        this.$emit("update:topClothes", "");
      } else {
        this.$emit("update:topClothes", value);
      }
    },
    toggleTopClothesColor(value) {
      if (this.localTopClothesColor === value) {
        this.$emit("update:topClothesColor", "");
      } else {
        this.$emit("update:topClothesColor", value);
      }
    },
    toggleBottomClothes(value) {
      if (this.localBottomClothes === value) {
        this.$emit("update:bottomClothes", "");
      } else {
        this.$emit("update:bottomClothes", value);
      }
    },
    toggleBottomClothesColor(value) {
      if (this.localBottomClothesColor === value) {
        this.$emit("update:bottomClothesColor", "");
      } else {
        this.$emit("update:bottomClothesColor", value);
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