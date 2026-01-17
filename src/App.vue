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

                <div class="d-flex align-center justify-space-between mb-3">
                  <div class="d-flex align-center">
                    <v-icon icon="mdi-lightbulb" class="mr-2"></v-icon>
                    <span class="text-h6">Presets</span>
                  </div>
                  <v-btn
                    color="success"
                    size="small"
                    prepend-icon="mdi-content-save"
                    @click="openSavePresetDialog"
                  >
                    Save Preset
                  </v-btn>
                </div>

                <div class="d-flex flex-wrap gap-2 mb-4">
                  <div
                    v-for="preset in allPresets"
                    :key="preset.id"
                    class="d-inline-flex align-center"
                  >
                    <v-chip
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
                    <v-menu v-if="preset.isCustom" location="bottom">
                      <template v-slot:activator="{ props }">
                        <v-btn
                          icon="mdi-dots-vertical"
                          size="x-small"
                          variant="text"
                          v-bind="props"
                          @click.stop
                          class="ml-1"
                          density="compact"
                        ></v-btn>
                      </template>
                      <v-list>
                        <v-list-item
                          prepend-icon="mdi-pencil"
                          title="Edit"
                          @click="openEditPresetDialog(preset)"
                        ></v-list-item>
                        <v-list-item
                          prepend-icon="mdi-delete"
                          title="Delete"
                          @click="confirmDeletePreset(preset)"
                        ></v-list-item>
                      </v-list>
                    </v-menu>
                  </div>
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

    <!-- Save Preset Dialog -->
    <v-dialog v-model="showSaveDialog" max-width="500">
      <v-card>
        <v-card-title>
          <span class="text-h5">Save Preset</span>
        </v-card-title>
        <v-card-text>
          <v-text-field
            v-model="newPresetName"
            label="Preset Name"
            variant="outlined"
            autofocus
            @keyup.enter="savePreset"
          ></v-text-field>
        </v-card-text>
        <v-card-actions>
          <v-spacer></v-spacer>
          <v-btn
            color="grey"
            variant="text"
            @click="closeSaveDialog"
          >
            Cancel
          </v-btn>
          <v-btn
            color="primary"
            variant="flat"
            @click="savePreset"
            :disabled="!newPresetName || newPresetName.trim() === ''"
          >
            Save
          </v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>

    <!-- Edit Preset Dialog -->
    <v-dialog v-model="showEditDialog" max-width="500">
      <v-card>
        <v-card-title>
          <span class="text-h5">Edit Preset</span>
        </v-card-title>
        <v-card-text>
          <v-text-field
            v-model="editingPresetName"
            label="Preset Name"
            variant="outlined"
            autofocus
            @keyup.enter="updatePreset"
          ></v-text-field>
        </v-card-text>
        <v-card-actions>
          <v-spacer></v-spacer>
          <v-btn
            color="grey"
            variant="text"
            @click="closeEditDialog"
          >
            Cancel
          </v-btn>
          <v-btn
            color="primary"
            variant="flat"
            @click="updatePreset"
            :disabled="!editingPresetName || editingPresetName.trim() === ''"
          >
            Update
          </v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>

    <!-- Delete Confirmation Dialog -->
    <v-dialog v-model="showDeleteDialog" max-width="500">
      <v-card>
        <v-card-title>
          <span class="text-h5">Delete Preset</span>
        </v-card-title>
        <v-card-text>
          Are you sure you want to delete preset "{{ deletingPreset?.name }}"? This action cannot be undone.
        </v-card-text>
        <v-card-actions>
          <v-spacer></v-spacer>
          <v-btn
            color="grey"
            variant="text"
            @click="closeDeleteDialog"
          >
            Cancel
          </v-btn>
          <v-btn
            color="error"
            variant="flat"
            @click="deletePreset"
          >
            Delete
          </v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>
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
      // Custom presets management
      customPresets: [],
      allPresets: [],
      showSaveDialog: false,
      showEditDialog: false,
      showDeleteDialog: false,
      newPresetName: "",
      editingPreset: null,
      editingPresetName: "",
      deletingPreset: null,
      nextCustomPresetId: 1000, // Start from 1000 to avoid conflicts with default presets
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
    // Helper: Convert value to code
    findCodeByValue(optionsArray, value) {
      if (!value) return null;
      const option = optionsArray.find((opt) => opt.value === value);
      return option ? option.code : value; // Fallback to value if not found
    },
    // Helper: Convert array of values to codes
    findCodesByValues(optionsArray, values) {
      if (!values || !Array.isArray(values)) return [];
      return values
        .map((value) => {
          const option = optionsArray.find((opt) => opt.value === value);
          return option ? option.code : value; // Fallback to value if not found
        })
        .filter((v) => v);
    },
    // Save current configuration as preset
    openSavePresetDialog() {
      this.newPresetName = "";
      this.showSaveDialog = true;
    },
    closeSaveDialog() {
      this.showSaveDialog = false;
      this.newPresetName = "";
    },
    savePreset() {
      if (!this.newPresetName || this.newPresetName.trim() === "") {
        return;
      }

      // Check if name already exists
      const nameExists = this.allPresets.some(
        (p) => p.name.toLowerCase() === this.newPresetName.trim().toLowerCase()
      );
      if (nameExists) {
        this.snackbarText = "Preset name already exists!";
        this.showSnackbar = true;
        return;
      }

      // Convert current values to codes
      const poseCodes = this.findCodesByValues(this.poseOptions, this.pose);
      const facialExpressionCodes = this.findCodesByValues(
        this.facialExpressionOptions,
        this.facialExpression
      );
      const bodyShapeCodes = this.findCodesByValues(
        this.bodyShapeOptions,
        this.bodyShape
      );
      const accessoriesCodes = this.findCodesByValues(
        this.accessoriesOptions,
        this.accessories
      );
      const topClothesCode = this.findCodeByValue(
        this.clothesTopOptions,
        this.topClothes
      );
      const bottomClothesCode = this.findCodeByValue(
        this.clothesBottomOptions,
        this.bottomClothes
      );
      const cameraShotCode = this.findCodeByValue(
        this.cameraShotOptions,
        this.cameraShot
      );
      const backgroundCode = this.findCodeByValue(
        this.backgroundOptions,
        this.background
      );

      const newPreset = {
        id: this.nextCustomPresetId++,
        name: this.newPresetName.trim(),
        icon: "mdi-bookmark",
        isCustom: true,
        pose: poseCodes,
        facialExpression: facialExpressionCodes,
        topClothes: topClothesCode || "",
        topClothesColor: this.topClothesColor || "",
        bottomClothes: bottomClothesCode || "",
        bottomClothesColor: this.bottomClothesColor || "",
        bodyShape: bodyShapeCodes,
        cameraShot: cameraShotCode || "",
        background: backgroundCode || "",
        accessories: accessoriesCodes,
      };

      this.customPresets.push(newPreset);
      this.updateAllPresets();
      this.saveCustomPresetsToLocalStorage();
      this.closeSaveDialog();
      this.snackbarText = "Preset saved successfully!";
      this.showSnackbar = true;
    },
    // Edit preset
    openEditPresetDialog(preset) {
      if (!preset.isCustom) {
        this.snackbarText = "Default presets cannot be edited!";
        this.showSnackbar = true;
        return;
      }
      this.editingPreset = preset;
      this.editingPresetName = preset.name;
      this.showEditDialog = true;
    },
    closeEditDialog() {
      this.showEditDialog = false;
      this.editingPreset = null;
      this.editingPresetName = "";
    },
    updatePreset() {
      if (!this.editingPreset || !this.editingPresetName || this.editingPresetName.trim() === "") {
        return;
      }

      // Check if name already exists (excluding current preset)
      const nameExists = this.allPresets.some(
        (p) =>
          p.id !== this.editingPreset.id &&
          p.name.toLowerCase() === this.editingPresetName.trim().toLowerCase()
      );
      if (nameExists) {
        this.snackbarText = "Preset name already exists!";
        this.showSnackbar = true;
        return;
      }

      // Update preset name
      const presetIndex = this.customPresets.findIndex(
        (p) => p.id === this.editingPreset.id
      );
      if (presetIndex !== -1) {
        this.customPresets[presetIndex].name = this.editingPresetName.trim();
        this.updateAllPresets();
        this.saveCustomPresetsToLocalStorage();
        this.closeEditDialog();
        this.snackbarText = "Preset updated successfully!";
        this.showSnackbar = true;
      }
    },
    // Delete preset
    confirmDeletePreset(preset) {
      if (!preset.isCustom) {
        this.snackbarText = "Default presets cannot be deleted!";
        this.showSnackbar = true;
        return;
      }
      this.deletingPreset = preset;
      this.showDeleteDialog = true;
    },
    closeDeleteDialog() {
      this.showDeleteDialog = false;
      this.deletingPreset = null;
    },
    deletePreset() {
      if (!this.deletingPreset) return;

      const presetIndex = this.customPresets.findIndex(
        (p) => p.id === this.deletingPreset.id
      );
      if (presetIndex !== -1) {
        this.customPresets.splice(presetIndex, 1);
        this.updateAllPresets();
        this.saveCustomPresetsToLocalStorage();
        
        // Clear selection if deleted preset was selected
        if (this.selectedPreset === this.deletingPreset.id) {
          this.selectedPreset = null;
        }
        
        this.closeDeleteDialog();
        this.snackbarText = "Preset deleted successfully!";
        this.showSnackbar = true;
      }
    },
    // Update all presets list (default + custom)
    updateAllPresets() {
      this.allPresets = [...this.presetOptions, ...this.customPresets];
    },
    // Save custom presets to localStorage
    saveCustomPresetsToLocalStorage() {
      try {
        const dataToSave = {
          presets: this.customPresets,
          nextId: this.nextCustomPresetId,
        };
        localStorage.setItem("imagex_custom_presets", JSON.stringify(dataToSave));
      } catch (error) {
        console.error("Error saving presets to localStorage:", error);
        this.snackbarText = "Error saving preset to storage";
        this.showSnackbar = true;
      }
    },
    // Load custom presets from localStorage
    loadCustomPresetsFromLocalStorage() {
      try {
        const saved = localStorage.getItem("imagex_custom_presets");
        if (saved) {
          const data = JSON.parse(saved);
          this.customPresets = data.presets || [];
          this.nextCustomPresetId = data.nextId || 1000;
          this.updateAllPresets();
        } else {
          this.customPresets = [];
          this.nextCustomPresetId = 1000;
          this.updateAllPresets();
        }
      } catch (error) {
        console.error("Error loading presets from localStorage:", error);
        this.customPresets = [];
        this.nextCustomPresetId = 1000;
        this.updateAllPresets();
      }
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
    // Load custom presets from localStorage
    this.loadCustomPresetsFromLocalStorage();
    
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

.preset-chip {
  position: relative;
}

.preset-chip :deep(.v-chip__close) {
  margin-left: 4px;
}
</style>
