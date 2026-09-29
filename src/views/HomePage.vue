<template>
  <ion-page>

    <ion-header>
      <ion-toolbar class="app-toolbar">
        <ion-icon :icon="shieldCheckmarkOutline" slot="start" class="toolbar-icon" />
        <ion-title>Cipher Tool</ion-title>
      </ion-toolbar>
    </ion-header>

    <ion-content class="ion-padding">

      <!-- Cipher selector card -->
      <ion-card class="cipher-card">
        <ion-card-header class="card-head">
          <div class="title-row">
            <div class="icon-chip chip-indigo">
              <ion-icon :icon="optionsOutline" />
            </div>
            <div>
              <ion-card-title>Encryption &amp; Decryption</ion-card-title>
            </div>
          </div>
        </ion-card-header>

        <ion-card-content>
          <ion-item lines="none" class="field">

            <ion-select v-model="selectedCipher" interface="popover">
              <ion-select-option value="caesar">Caesar Cipher</ion-select-option>
              <ion-select-option value="vigenere">Vigenère Cipher</ion-select-option>
            </ion-select>
          </ion-item>
        </ion-card-content>
      </ion-card>

      <!-- Cipher panels (animated swap) -->
      <Transition name="fade-slide" mode="out-in">
        <CaesarCipher v-if="selectedCipher === 'caesar'" />
        <VigenereCipher v-else-if="selectedCipher === 'vigenere'" />
      </Transition>

    </ion-content>

  </ion-page>
</template>

<script setup>
import { ref } from 'vue'

import {
  IonPage,
  IonHeader,
  IonToolbar,
  IonTitle,
  IonContent,
  IonCard,
  IonCardHeader,
  IonCardTitle,
  IonCardSubtitle,
  IonCardContent,
  IonItem,
  IonLabel,
  IonSelect,
  IonSelectOption,
  IonIcon
} from '@ionic/vue'

import { shieldCheckmarkOutline, optionsOutline } from 'ionicons/icons'

import CaesarCipher from '@/components/CaesarCipher.vue'
import VigenereCipher from '@/components/VigenereCipher.vue'

const selectedCipher = ref('caesar')
</script>

<style scoped>
/* ── Header ─────────────────────────────── */
ion-toolbar.app-toolbar {
  --background: linear-gradient(135deg, #4f46e5 0%, #7c3aed 55%, #9333ea 100%);
  --color: #ffffff;
  --border-width: 0;
}

.toolbar-icon {
  font-size: 22px;
  margin-left: 16px;
  margin-right: 4px;
}

ion-title {
  font-weight: 800;
  letter-spacing: 0.5px;
}

/* ── Page background ────────────────────── */
ion-content {
  --background: linear-gradient(180deg, #eef2ff 0%, #f8fafc 60%, #fdf2f8 100%);
}

@media (prefers-color-scheme: dark) {
  ion-content {
    --background: linear-gradient(180deg, #0b1120 0%, #111827 60%, #1e1b4b 100%);
  }
}

/* ── Card ───────────────────────────────── */
.cipher-card {
  border-radius: 20px;
  box-shadow: 0 12px 32px rgba(2, 6, 23, 0.10);
  margin: 4px 0 20px;
}

@media (prefers-color-scheme: dark) {
  .cipher-card {
    box-shadow: 0 12px 32px rgba(0, 0, 0, 0.45);
  }
}

.card-head {
  padding-bottom: 4px;
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

.chip-indigo {
  background: linear-gradient(135deg, #6366f1, #a855f7);
  box-shadow: 0 6px 16px rgba(124, 58, 237, 0.35);
}

ion-card-title {
  font-size: 19px;
  font-weight: 800;
  letter-spacing: 0.2px;
}

ion-card-subtitle {
  font-size: 13px;
  margin-top: 2px;
}

/* ── Select field ───────────────────────── */
.field {
  --background: rgba(var(--ion-color-light-rgb), 0.55);
  border-radius: 14px;
  --padding-start: 14px;
  --inner-padding-end: 14px;
}

ion-select {
  font-weight: 600;
}

/* ── Footer note ────────────────────────── */
.footer-note {
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 6px;
  color: var(--ion-color-medium);
  font-size: 12.5px;
  margin: 4px 0 24px;
  text-align: center;
}

.footer-note ion-icon {
  font-size: 15px;
}

/* ── Cipher swap animation ──────────────── */
.fade-slide-enter-active,
.fade-slide-leave-active {
  transition: opacity 0.25s ease, transform 0.25s ease;
}

.fade-slide-enter-from {
  opacity: 0;
  transform: translateY(12px);
}

.fade-slide-leave-to {
  opacity: 0;
  transform: translateY(-12px);
}
</style>