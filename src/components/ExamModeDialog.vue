<script lang="ts" setup>
import { VDialog } from 'vuetify/components/VDialog';
import { VCard, VCardTitle } from 'vuetify/components/VCard';
import { VBtn } from 'vuetify/components/VBtn';
import { VTextField } from 'vuetify/components/VTextField';
import { VDivider } from 'vuetify/components/VDivider';
import { mdiClose, mdiPlus, mdiDelete } from '@mdi/js';

import useStore from '../store/index';

const store = useStore();

const generateId = () => Math.random().toString(36).substring(2, 9);

const addExamPeriod = () => {
  store.preferences.examPeriods.push({
    id: generateId(),
    subject: '',
    startTime: '08:00',
    endTime: '10:00',
  });
};

const removeExamPeriod = (id: string) => {
  const index = store.preferences.examPeriods.findIndex(e => e.id === id);
  if (index !== -1) {
    store.preferences.examPeriods.splice(index, 1);
  }
};
</script>

<template>
  <v-dialog v-model="store.examDialogOpen" max-width="500">
    <v-card>
      <template #title><v-card-title>考试模式设置</v-card-title></template>
      <template #append>
        <v-btn :icon="mdiClose" color="error" @click="store.examDialogOpen = false;"></v-btn>
      </template>
      <div class="exam-content">
        <div v-if="store.preferences.examPeriods.length === 0" class="text-center pa-4 text-grey">
          暂无考试时间段，点击下方按钮添加
        </div>
        <div v-for="(exam, index) in store.preferences.examPeriods" :key="exam.id" class="exam-item">
          <div class="exam-header">
            <span class="exam-index">时间段 {{ index + 1 }}</span>
            <v-btn :icon="mdiDelete" color="error" variant="text" size="small" @click="removeExamPeriod(exam.id)"></v-btn>
          </div>
          <v-text-field
            v-model="exam.subject"
            label="考试科目"
            placeholder="如：语文、数学"
          ></v-text-field>
          <div class="time-inputs">
            <v-text-field
              v-model="exam.startTime"
              label="开始时间"
              type="time"
              hide-details
            ></v-text-field>
            <span class="time-separator">至</span>
            <v-text-field
              v-model="exam.endTime"
              label="结束时间"
              type="time"
              hide-details
            ></v-text-field>
          </div>
          <v-divider v-if="index < store.preferences.examPeriods.length - 1" class="mt-4"></v-divider>
        </div>
      </div>
      <v-divider></v-divider>
      <div class="exam-actions">
        <v-btn :prepend-icon="mdiPlus" color="primary" variant="text" @click="addExamPeriod">添加时间段</v-btn>
        <v-spacer></v-spacer>
        <v-btn color="primary" variant="text" @click="store.examDialogOpen = false">完成</v-btn>
      </div>
    </v-card>
  </v-dialog>
</template>

<style scoped>
.exam-content {
  max-height: 400px;
  overflow-y: auto;
  padding: 0 1rem;
}

.exam-item {
  padding: 1rem 0;
}

.exam-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 0.5rem;
}

.exam-index {
  font-weight: 500;
}

.time-inputs {
  display: flex;
  align-items: center;
  gap: 8px;
}

.time-inputs > :first-child,
.time-inputs > :last-child {
  flex: 1;
}

.time-separator {
  color: #666;
  white-space: nowrap;
}

.exam-actions {
  display: flex;
  padding: 0.5rem 1rem;
}
</style>