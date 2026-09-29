<template>
  <ion-card class="cipher-card">
    <ion-card-header class="card-head">
      <div class="title-row">
        <div class="icon-chip chip-sky">
          <ion-icon :icon="keyOutline" />
        </div>
        <div>
          <ion-card-title>Vigenère Cipher</ion-card-title>
          <ion-card-subtitle>
            Encrypt using a keyword
          </ion-card-subtitle>
        </div>
      </div>
    </ion-card-header>

    <ion-card-content>

      <ion-item lines="none" class="field">
        <ion-label position="stacked">
          Text
        </ion-label>

        <ion-textarea
          v-model="text"
          placeholder="Enter text"
          :rows="5"
        ></ion-textarea>
      </ion-item>

      <ion-item lines="none" class="field">
        <ion-label position="stacked">
          Keyword
        </ion-label>

        <ion-input
          v-model="keyword"
          placeholder="Enter keyword"
        ></ion-input>
      </ion-item>

      <div class="button-container">
        <ion-button expand="block" class="encrypt-btn" @click="encrypt">
          <ion-icon slot="start" :icon="lockClosedOutline" />
          Encrypt
        </ion-button>

        <ion-button
          expand="block"
          fill="outline"
          class="decrypt-btn"
          @click="decrypt"
        >
          <ion-icon slot="start" :icon="lockOpenOutline" />
          Decrypt
        </ion-button>
      </div>

      <Transition name="pop">
        <ion-item v-if="result" lines="none" class="result-box">
          <ion-label position="stacked" class="result-label">
            <ion-icon :icon="checkmarkDoneOutline" />
            Result
          </ion-label>

          <ion-textarea
            :value="result"
            :rows="5"
            readonly
          ></ion-textarea>
        </ion-item>
      </Transition>

    </ion-card-content>
  </ion-card>
</template>

<script setup>
import { ref } from 'vue'

import {
  IonCard,
  IonCardHeader,
  IonCardTitle,
  IonCardSubtitle,
  IonCardContent,
  IonItem,
  IonLabel,
  IonInput,
  IonTextarea,
  IonButton,
  IonIcon
} from '@ionic/vue'

import {
  keyOutline,
  lockClosedOutline,
  lockOpenOutline,
  checkmarkDoneOutline
} from 'ionicons/icons'

const text = ref('')
const keyword = ref('')
const result = ref('')

function vigenereCipher(input, key, decryptMode = false) {
  if (!key) {
    return input
  }

  let output = ''
  let keyIndex = 0

  const cleanKey = key.replace(/[^a-zA-Z]/g, '').toUpperCase()

  if (!cleanKey) {
    return input
  }

  for (let i = 0; i < input.length; i++) {
    const char = input[i]

    if (char >= 'A' && char <= 'Z') {
      let shift =
        cleanKey.charCodeAt(keyIndex % cleanKey.length) - 65

      if (decryptMode) {
        shift = -shift
      }

      const code =
        ((char.charCodeAt(0) - 65 + shift) % 26 + 26) % 26

      output += String.fromCharCode(code + 65)

      keyIndex++
    }

    else if (char >= 'a' && char <= 'z') {
      let shift =
        cleanKey.charCodeAt(keyIndex % cleanKey.length) - 65

      if (decryptMode) {
        shift = -shift
      }

      const code =
        ((char.charCodeAt(0) - 97 + shift) % 26 + 26) % 26

      output += String.fromCharCode(code + 97)

      keyIndex++
    }

    else {
      output += char
    }
  }

  return output
}

function encrypt() {
  result.value = vigenereCipher(
    text.value,
    keyword.value,
    false
  )
}

function decrypt() {
  result.value = vigenereCipher(
    text.value,
    keyword.value,
    true
  )
}
</script>

<style scoped>
/* ── Card ───────────────────────────────── */
.cipher-card {
  border-radius: 20px;
  box-shadow: 0 12px 32px rgba(2, 6, 23, 0.10);
  margin: 0 0 20px;
  overflow: hidden;
}

@media (prefers-color-scheme: dark) {
  .cipher-card {
    box-shadow: 0 12px 32px rgba(0, 0, 0, 0.45);
  }
}

.card-head {
  padding-bottom: 6px;
}

.title-row {
  display: flex;
  align-items: center;
  gap: 14px;
  padding: 4px 2px;
}

.icon-chip {
  width: 46px;
  height: 46px;
  border-radius: 14px;
  flex-shrink: 0;
  display: flex;
  align-items: center;
  justify-content: center;
  color: #ffffff;
  font-size: 24px;
}

.chip-sky {
  background: linear-gradient(135deg, #0ea5e9, #6366f1);
  box-shadow: 0 6px 16px rgba(14, 165, 233, 0.35);
}

ion-card-title {
  font-size: 19px;
  font-weight: 800;
}

ion-card-subtitle {
  font-size: 13px;
  margin-top: 2px;
}

/* ── Fields ─────────────────────────────── */
.field {
  --background: rgba(var(--ion-color-light-rgb), 0.55);
  border-radius: 14px;
  margin-bottom: 12px;
  --padding-start: 14px;
  --inner-padding-end: 14px;
}

.hint {
  margin: 2px 6px 0;
  font-size: 12.5px;
  font-style: italic;
  color: var(--ion-color-medium);
}

/* ── Buttons ────────────────────────────── */
.button-container {
  display: flex;
  gap: 10px;
  margin-top: 20px;
}

.button-container ion-button {
  flex: 1;
  margin: 0;
}

.encrypt-btn {
  --background: linear-gradient(135deg, #22c55e, #16a34a);
  --background-activated: linear-gradient(135deg, #16a34a, #15803d);
  --border-radius: 14px;
  --box-shadow: 0 6px 18px rgba(34, 197, 94, 0.35);
  --color: #ffffff;
  font-weight: 700;
  letter-spacing: 0.3px;
}

.decrypt-btn {
  --border-radius: 14px;
  --border-color: var(--ion-color-danger);
  --color: var(--ion-color-danger);
  --background: rgba(var(--ion-color-danger-rgb), 0.06);
  --background-activated: rgba(var(--ion-color-danger-rgb), 0.15);
  font-weight: 700;
  letter-spacing: 0.3px;
}

/* ── Result box ─────────────────────────── */
.result-box {
  --background: rgba(var(--ion-color-success-rgb), 0.08);
  border: 1.5px solid rgba(var(--ion-color-success-rgb), 0.45);
  border-radius: 14px;
  margin-top: 18px;
  --padding-start: 14px;
}

.result-label {
  font-weight: 700;
}

.result-label ion-icon {
  vertical-align: -2px;
  margin-right: 4px;
  color: var(--ion-color-success);
}

ion-textarea::part(textarea) {
  font-family: ui-monospace, 'SF Mono', 'Cascadia Mono', 'Courier New', monospace;
  letter-spacing: 0.5px;
}

/* ── Result pop-in animation ────────────── */
.pop-enter-active {
  transition: opacity 0.25s ease, transform 0.25s ease;
}

.pop-leave-active {
  transition: opacity 0.2s ease, transform 0.2s ease;
}

.pop-enter-from {
  opacity: 0;
  transform: translateY(8px) scale(0.98);
}

.pop-leave-to {
  opacity: 0;
  transform: translateY(-8px) scale(0.98);
}
</style>