<template>
  <v-app>
    <v-app-bar color="primary" dark elevation="4">
      <v-app-bar-title class="d-flex align-center">
        <v-icon icon="mdi-text-box-multiple" class="mr-2"></v-icon>
        ImageX
      </v-app-bar-title>
    </v-app-bar>

    <v-main>


      <!-- ----------------------------------------------------------------- -->

      <v-container class="pa-6" style="max-width: 1200px">
        <v-row>
          <!-- ------------------------------------------------------------- -->
          <!-- Konfigurasi Prompt -->

          <v-col cols="12" md="6">
            <v-card elevation="2" class="h-100">
              <v-card-title class="d-flex align-center bg-primary text-white">
                <v-icon icon="mdi-cog" class="mr-2"></v-icon>
                Prompt Configurator
              </v-card-title>

              <v-card-text class="pa-4">
                <!-- ------------------------------------------------------- -->
                <!-- Prompt Preset -->

                <div class="d-flex align-center mb-3">
                  <v-icon icon="mdi-lightbulb" class="mr-2"></v-icon>
                  <span class="text-h6">Presets</span>
                </div>

                <div class="d-flex flex-wrap gap-2 mb-4">
                  <v-chip
                    v-for="preset in presetOptions"
                    :key="preset.id"
                    :color="
                      selectedPreset === preset.id ? 'primary' : 'default'
                    "
                    :variant="
                      selectedPreset === preset.id ? 'flat' : 'outlined'
                    "
                    @click="applyPreset(preset)"
                    class="cursor-pointer"
                    size="small"
                  >
                    <v-icon v-if="preset.icon" :icon="preset.icon" class="mr-1"></v-icon>
                    {{ preset.name }}
                  </v-chip>
                </div>
              </v-card-text>

              <v-divider class="my-4"></v-divider>

              <v-card-text class="pa-4">
                <!-- ------------------------------------------------------- -->
                <!-- Pose -->

                <PoseView v-model="pose" />

                <v-divider class="my-4"></v-divider>

                <!-- ------------------------------------------------------- -->
                <!-- Facial Expression -->

                <!-- ------------------------------------------------------- -->
                <!-- Facial Expression -->

                <FacialExpressionView v-model="facialExpression" />

                <v-divider class="my-4"></v-divider>

                <!-- ------------------------------------------------------- -->
                <!-- Clothes -->

                <!-- ------------------------------------------------------- -->
                <!-- Clothes -->

                <ClothesView
                  v-model:top-clothes="topClothes"
                  v-model:top-clothes-color="topClothesColor"
                  v-model:bottom-clothes="bottomClothes"
                  v-model:bottom-clothes-color="bottomClothesColor"
                />

                <v-divider class="my-4"></v-divider>

                <!-- ------------------------------------------------------- -->
                <!-- Body Shape -->

                <!-- ------------------------------------------------------- -->
                <!-- Body Shape -->

                <BodyShapeView v-model="bodyShape" />

                <v-divider class="my-4"></v-divider>

                <!-- ------------------------------------------------------- -->
                <!-- Camera Shot -->

                <!-- ------------------------------------------------------- -->
                <!-- Camera Shot -->

                <CameraShotView v-model="cameraShot" />

                <v-divider class="my-4"></v-divider>

                <!-- ------------------------------------------------------- -->
                <!-- Background -->

                <!-- ------------------------------------------------------- -->
                <!-- Background -->

                <BackgroundView v-model="background" />

                <v-divider class="my-4"></v-divider>

                <!-- ------------------------------------------------------- -->
                <!-- Accessories -->

                <!-- ------------------------------------------------------- -->
                <!-- Accessories -->

                <AccessoriesView v-model="accessories" />
              </v-card-text>
            </v-card>
          </v-col>

          <!-- ------------------------------------------------------------- -->
          <!-- Generated Prompt -->

          <v-col>
            <v-card elevation="2" class="h-100">
              <v-card-title class="d-flex align
              -center bg-primary text-white">
                <v-icon icon="mdi-cog" class="mr-2"></v-icon>
                Generated Prompt
              </v-card-title>

              <v-card-text class="pa-4">
                <div class="d-flex gap-2 mb-3">
                  <v-btn color="primary" @click="copyPrompt">
                    <v-icon icon="mdi-content-copy" class="mr-2"></v-icon>
                    Copy Prompt
                  </v-btn>
                  <v-btn
                    color="secondary"
                    variant="outlined"
                    prepend-icon="mdi-refresh"
                    @click="resetPrompt"
                  >
                    Reset
                  </v-btn>
                </div>
                <v-textarea
                  v-model="prompt"
                  rows="100"
                  variant="outlined"
                ></v-textarea>
                
              </v-card-text>
            </v-card>
          </v-col>
        </v-row>
      </v-container>

      <v-container class="pa-6" style="max-width: 1200px">
        <v-divider class="my-4"></v-divider>
      </v-container>
    </v-main>

    <v-footer app color="primary" dark class="justify-center">
      <span>ImageX - Image transformation prompts generator</span>
    </v-footer>

    <!-- Snackbar for notifications -->
    <v-snackbar
      v-model="showSnackbar"
      :timeout="3000"
      color="success"
      location="top"
    >
      {{ snackbarText }}
      <template v-slot:actions>
        <v-btn
          color="white"
          variant="text"
          @click="showSnackbar = false"
        >
          Close
        </v-btn>
      </template>
    </v-snackbar>
  </v-app>
</template>

<script>
import { poseOptions } from "./options/poseOptions.js";
import { clothesTopOptions } from "./options/clothesTopOptions.js";
import { clothesBottomOptions } from "./options/clothesBottomOptions.js";
import { cameraShotOptions } from "./options/cameraShotOptions.js";
import { backgroundOptions } from "./options/backgroundOptions.js";
import { colorOptions } from "./options/colorOptions.js";
import { accessoriesOptions } from "./options/accessoriesOptions.js";
import { presetOptions } from "./options/presetOptions.js";
import { bodyShapeOptions } from "./options/bodyShapeOptions.js";
import { facialExpressionOptions } from "./options/facialExpressionOptions.js";
import PoseView from "./components/PoseView.vue";
import FacialExpressionView from "./components/FacialExpressionView.vue";
import ClothesView from "./components/ClothesView.vue";
import BodyShapeView from "./components/BodyShapeView.vue";
import CameraShotView from "./components/CameraShotView.vue";
import BackgroundView from "./components/BackgroundView.vue";
import AccessoriesView from "./components/AccessoriesView.vue";

export default {
  name: "App",
  components: {
    PoseView,
    FacialExpressionView,
    ClothesView,
    BodyShapeView,
    CameraShotView,
    BackgroundView,
    AccessoriesView,
  },
  data() {
    return {
      prompt: "",
      pose: [],
      topClothes: "",
      topClothesColor: "",
      bottomClothes: "",
      bottomClothesColor: "",
      bodyShape: [],
      cameraShot: "",
      background: "",
      accessories: [],
      facialExpression: [],
      selectedPreset: null,
      showSnackbar: false,
      snackbarText: "",
      poseOptions,
      clothesTopOptions,
      clothesBottomOptions,
      cameraShotOptions,
      backgroundOptions,
      colorOptions,
      accessoriesOptions,
      presetOptions,
      bodyShapeOptions,
      facialExpressionOptions,
      activeTab: "pose",
    };
  },

  methods: {
    togglePose(value) {
      if (!this.pose) {
        this.pose = [];
      }
      const index = this.pose.indexOf(value);
      if (index > -1) {
        this.pose.splice(index, 1);
      } else {
        this.pose.push(value);
      }
    },
    handlePoseSelected(value) {
      if (!this.pose) {
        this.pose = [];
      }
      if (!this.pose.includes(value)) {
        this.pose.push(value);
      }
    },
    handlePoseDeselected(value) {
      if (!this.pose) {
        this.pose = [];
      }
      const index = this.pose.indexOf(value);
      if (index > -1) {
        this.pose.splice(index, 1);
      }
    },
    toggleAccessories(value) {
      if (!this.accessories) {
        this.accessories = [];
      }
      const index = this.accessories.indexOf(value);
      if (index > -1) {
        this.accessories.splice(index, 1);
      } else {
        this.accessories.push(value);
      }
    },
    toggleTopClothesColor(value) {
      if (this.topClothesColor === value) {
        this.topClothesColor = "";
      } else {
        this.topClothesColor = value;
      }
    },
    toggleBottomClothesColor(value) {
      if (this.bottomClothesColor === value) {
        this.bottomClothesColor = "";
      } else {
        this.bottomClothesColor = value;
      }
    },
    toggleTopClothes(value) {
      if (this.topClothes === value) {
        this.topClothes = "";
      } else {
        this.topClothes = value;
      }
    },
    toggleBottomClothes(value) {
      if (this.bottomClothes === value) {
        this.bottomClothes = "";
      } else {
        this.bottomClothes = value;
      }
    },
    toggleCameraShot(value) {
      if (this.cameraShot === value) {
        this.cameraShot = "";
      } else {
        this.cameraShot = value;
      }
    },
    toggleBackground(value) {
      if (this.background === value) {
        this.background = "";
      } else {
        this.background = value;
      }
    },
    toggleBodyShape(value) {
      if (!this.bodyShape) {
        this.bodyShape = [];
      }
      const index = this.bodyShape.indexOf(value);
      if (index > -1) {
        this.bodyShape.splice(index, 1);
      } else {
        this.bodyShape.push(value);
      }
    },
    toggleFacialExpression(value) {
      if (!this.facialExpression) {
        this.facialExpression = [];
      }
      const index = this.facialExpression.indexOf(value);
      if (index > -1) {
        this.facialExpression.splice(index, 1);
      } else {
        this.facialExpression.push(value);
      }
    },
    findOptionByCode(optionsArray, code) {
      if (!code) return null;
      return optionsArray.find((opt) => opt.code === code);
    },
    resolvePresetValue(preset, field, optionsArray) {
      // Support both code and direct value for backward compatibility
      const codeOrValue = preset[field];
      if (!codeOrValue) return "";

      // If it's an array (for pose/accessories)
      if (Array.isArray(codeOrValue)) {
        return codeOrValue
          .map((item) => {
            const option = this.findOptionByCode(optionsArray, item);
            return option ? option.value : item; // Fallback to direct value if not found
          })
          .filter((v) => v);
      }

      // If it's a string
      const option = this.findOptionByCode(optionsArray, codeOrValue);
      return option ? option.value : codeOrValue; // Fallback to direct value if not found
    },
    applyPreset(preset) {
      this.selectedPreset = preset.id;

      // Resolve pose codes to values
      const poseCodes = Array.isArray(preset.pose)
        ? preset.pose
        : preset.pose
        ? [preset.pose]
        : [];
      this.pose = poseCodes
        .map((code) => this.resolvePresetValue({ pose: code }, "pose", this.poseOptions))
        .filter((v) => v);

      // Resolve other fields
      this.topClothes = this.resolvePresetValue(
        preset,
        "topClothes",
        this.clothesTopOptions
      );
      this.topClothesColor = preset.topClothesColor || "";
      this.bottomClothes = this.resolvePresetValue(
        preset,
        "bottomClothes",
        this.clothesBottomOptions
      );
      this.bottomClothesColor = preset.bottomClothesColor || "";
      
      // Resolve bodyShape codes to values
      const bodyShapeCodes = Array.isArray(preset.bodyShape)
        ? preset.bodyShape
        : preset.bodyShape
        ? [preset.bodyShape]
        : [];
      this.bodyShape = bodyShapeCodes
        .map((code) => this.resolvePresetValue({ bodyShape: code }, "bodyShape", this.bodyShapeOptions))
        .filter((v) => v);
      
      this.cameraShot = this.resolvePresetValue(
        preset,
        "cameraShot",
        this.cameraShotOptions
      );
      this.background = this.resolvePresetValue(
        preset,
        "background",
        this.backgroundOptions
      );
      // Resolve facialExpression codes to values
      if (preset.facialExpression && Array.isArray(preset.facialExpression)) {
        this.facialExpression = preset.facialExpression
          .map((code) =>
            this.resolvePresetValue(
              { facialExpression: code },
              "facialExpression",
              this.facialExpressionOptions
            )
          )
          .filter((v) => v);
      } else if (preset.facialExpression) {
        // Support single value for backward compatibility
        const codeOrValue = preset.facialExpression;
        const option = this.findOptionByCode(this.facialExpressionOptions, codeOrValue);
        this.facialExpression = option ? [option.value] : [codeOrValue];
      } else {
        this.facialExpression = [];
      }

      // Resolve accessories codes to values
      if (preset.accessories && Array.isArray(preset.accessories)) {
        this.accessories = preset.accessories
          .map((code) =>
            this.resolvePresetValue(
              { accessories: code },
              "accessories",
              this.accessoriesOptions
            )
          )
          .filter((v) => v);
      }

      // Prompt akan otomatis ter-generate melalui watchers
    },
    generatePrompt() {
      const clothesParts = [];

      // Build top clothes with color
      let topClothesStr = "";
      if (this.topClothes) {
        topClothesStr = this.topClothes;
        if (this.topClothesColor) {
          topClothesStr = `${this.topClothesColor} ${topClothesStr}`;
        }
        clothesParts.push(topClothesStr);
      }

      // Build bottom clothes with color
      let bottomClothesStr = "";
      if (this.bottomClothes) {
        bottomClothesStr = this.bottomClothes;
        if (this.bottomClothesColor) {
          bottomClothesStr = `${this.bottomClothesColor} ${bottomClothesStr}`;
        }
        clothesParts.push(bottomClothesStr);
      }

      const clothes = clothesParts.length > 0 ? clothesParts.join(" and ") : "";

      let generatedPrompt = "Edit this photo:\n\n";
      
      // Base Requirements
      const baseRequirements = [
        "remove all text",
        "remove all accessories",
        "dont change face feature",
        "photorealistic",
        "ultra detailed",
        "8k resolution"
      ];
      
      if (baseRequirements.length > 0) {
        generatedPrompt += "Base Requirements:\n";
        baseRequirements.forEach(req => {
          generatedPrompt += `- ${req}\n`;
        });
        generatedPrompt += "\n";
      }
      
      // Pose
      if (this.pose && this.pose.length > 0) {
        generatedPrompt += "Pose:\n";
        this.pose.forEach(pose => {
          generatedPrompt += `- ${pose}\n`;
        });
        generatedPrompt += "\n";
      }
      
      // Facial Expression
      if (this.facialExpression && this.facialExpression.length > 0) {
        generatedPrompt += "Facial Expression:\n";
        this.facialExpression.forEach(expression => {
          generatedPrompt += `- ${expression}\n`;
        });
        generatedPrompt += "\n";
      }
      
      // Body Shape
      if (this.bodyShape && this.bodyShape.length > 0) {
        generatedPrompt += "Body Shape:\n";
        this.bodyShape.forEach(shape => {
          generatedPrompt += `- ${shape}\n`;
        });
        generatedPrompt += "\n";
      }
      
      // Clothes
      if (clothes) {
        generatedPrompt += "Clothes:\n";
        generatedPrompt += `- ${clothes}\n`;
        generatedPrompt += "\n";
      }
      
      // Accessories
      if (this.accessories && this.accessories.length > 0) {
        generatedPrompt += "Accessories:\n";
        this.accessories.forEach(accessory => {
          generatedPrompt += `- ${accessory}\n`;
        });
        generatedPrompt += "\n";
      }
      
      // Camera Shot
      if (this.cameraShot) {
        generatedPrompt += "Camera Shot:\n";
        generatedPrompt += `- ${this.cameraShot}\n`;
        generatedPrompt += "\n";
      }
      
      // Background
      if (this.background) {
        generatedPrompt += "Background:\n";
        generatedPrompt += `- background with ${this.background}\n`;
        generatedPrompt += "\n";
      }

      // Remove trailing newlines
      generatedPrompt = generatedPrompt.trim();

      this.prompt = generatedPrompt;
    },
    copyPrompt() {
      navigator.clipboard.writeText(this.prompt).then(() => {
        this.snackbarText = "Prompt copied to clipboard!";
        this.showSnackbar = true;
      }).catch(() => {
        this.snackbarText = "Failed to copy prompt";
        this.showSnackbar = true;
      });
    },
    resetPrompt() {
      // Reset preset selection
      this.selectedPreset = null;

      // Reset semua nilai ke default (index 0)
      if (this.poseOptions.length > 0) {
        this.pose = [this.poseOptions[0].value];
      } else {
        this.pose = [];
      }

      if (this.facialExpressionOptions.length > 0) {
        this.facialExpression = [this.facialExpressionOptions[0].value];
      } else {
        this.facialExpression = [];
      }

      if (this.clothesTopOptions.length > 0) {
        this.topClothes = this.clothesTopOptions[0].value;
      } else {
        this.topClothes = "";
      }

      this.topClothesColor = "";

      if (this.clothesBottomOptions.length > 0) {
        this.bottomClothes = this.clothesBottomOptions[0].value;
      } else {
        this.bottomClothes = "";
      }

      this.bottomClothesColor = "";

      if (this.bodyShapeOptions.length > 0) {
        this.bodyShape = [this.bodyShapeOptions[0].value];
      } else {
        this.bodyShape = [];
      }

      if (this.cameraShotOptions.length > 0) {
        this.cameraShot = this.cameraShotOptions[0].value;
      } else {
        this.cameraShot = "";
      }

      if (this.backgroundOptions.length > 0) {
        this.background = this.backgroundOptions[0].value;
      } else {
        this.background = "";
      }

      this.accessories = [];

      this.facialExpression = [];

      // Generate prompt dengan nilai default
      this.generatePrompt();
    },
    init() {
      this.pose = "standing";
      this.topClothes = "";
      this.bottomClothes = "";
      this.cameraShot = "front";
      this.background = "studio";
    },
  },
  watch: {
    pose: {
      handler() {
        this.generatePrompt();
      },
      deep: true,
    },
    topClothes() {
      this.generatePrompt();
    },
    bottomClothes() {
      this.generatePrompt();
    },
    cameraShot() {
      this.generatePrompt();
    },
    background() {
      this.generatePrompt();
    },
    topClothesColor() {
      this.generatePrompt();
    },
    bottomClothesColor() {
      this.generatePrompt();
    },
    bodyShape: {
      handler() {
        this.generatePrompt();
      },
      deep: true,
    },
    accessories: {
      handler() {
        this.generatePrompt();
      },
      deep: true,
    },
    facialExpression: {
      handler() {
        this.generatePrompt();
      },
      deep: true,
    },
  },
  created() {
    // Set default values (index 0) untuk masing-masing pilihan
    if (this.poseOptions.length > 0) {
      this.pose = [this.poseOptions[0].value];
    }
    if (this.facialExpressionOptions.length > 0) {
      this.facialExpression = [this.facialExpressionOptions[0].value];
    }
    if (this.clothesTopOptions.length > 0) {
      this.topClothes = this.clothesTopOptions[0].value;
    }
    if (this.clothesBottomOptions.length > 0) {
      this.bottomClothes = this.clothesBottomOptions[0].value;
    }
    if (this.bodyShapeOptions.length > 0) {
      this.bodyShape = [this.bodyShapeOptions[0].value];
    }
    if (this.cameraShotOptions.length > 0) {
      this.cameraShot = this.cameraShotOptions[0].value;
    }
    if (this.backgroundOptions.length > 0) {
      this.background = this.backgroundOptions[0].value;
    }

    // Generate prompt dengan nilai default
    this.generatePrompt();
  },
};
</script>

<style scoped>
.cursor-pointer {
  cursor: pointer;
}

.gap-2 {
  gap: 8px;
}
</style>
