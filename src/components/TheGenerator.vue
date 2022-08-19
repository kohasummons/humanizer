<template>
  <div class="container">
    <div class="generator">
      <div class="generator__header">
        <p class="guide-text">
          Paste your ABI code into the editor. Select your format type. Click
          generate
        </p>
      </div>
      <div class="generator__editor">
        <code-mirror ref="hinge" :format-type="formatType" />
        <div class="generator__options">
          <form>
            <p class="radio__label">Format Type</p>
            <div class="form-controls">
              <span>
                <input
                  name="formattype"
                  type="radio"
                  id="full"
                  value="full"
                  v-model="formatType"
                />
                <label for="full" style="color: white">Full</label>
              </span>
              <span>
                <input
                  name="formattype"
                  type="radio"
                  id="minimal"
                  value="minimal"
                  v-model="formatType"
                />
                <label for="minimal" style="color: white">Minimal</label>
              </span>
            </div>
            <div class="btn-group">
              <button class="btn btn-generate" @click.prevent="handleGenerate">Generate</button>
              <button class="btn btn-copy" @click.prevent="handleCopy">Copy</button>
            </div>
          </form>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import CodeMirror from "./CodeMirror.vue";
import { ref } from "vue";

const hinge = ref();
const formatType = ref("full");

function handleGenerate() {
  // hinge.value.replaceEditorContent();
  hinge.value.handleAbiGenerator();
}

function handleCopy() {
  hinge.value.handleCopy();
}
</script>

<style scoped lang="scss">
.generator {
  margin-top: 2rem;
}

.generator__editor {
  padding-top: 1em;
}

.generator__options {
  padding-top: 0.75em;
}

.btn-group {
  display: grid;
  grid-template-rows: 1fr 1fr;
  gap: 0.25em;
}

.btn-copy{
    background-color: #0d0d0e;

}

.guide-text {
  font-size: 0.75em;
  font-weight: 500;
  color: rgb(151, 151, 151);
}

.radio__label {
}

.form-controls {
  margin-bottom: 0.25em;
  & span {
    display: block;
    & label {
      display: inline-block;
      margin-left: 0.25em;
    }
  }
}

@media screen and (min-width: 768px) {
  .generator__editor {
    padding: 0;
    display: grid;
    grid-template-columns: auto 1fr;
  }

  .generator__options {
    padding: 0.75em;
  }
}
</style>