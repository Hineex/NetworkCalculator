<script setup lang="ts">
import { UiButton } from 'netkit-ui';
import { UiField } from 'netkit-ui';
import { UiSelect } from 'netkit-ui';
import { UiInput } from 'netkit-ui';
import { ref } from 'vue';
import { SUBNET_MASKS } from './masks';

const ip = ref('');
const mask = ref(SUBNET_MASKS[0]);
const isShowResult = ref(false);

function showResult() {
  isShowResult.value = true;
}

function isIpValid(ip: string): boolean {
  return (
    /^(\d{1,3})\.(\d{1,3})\.(\d{1,3})\.(\d{1,3})$/.test(ip) &&
    ip.split('.').every((octet) => Number(octet) <= 255)
  );
}

function getNetworkAdress(ip: string, mask: string): string {
  const ipOctets = ip.split('.');
  const maskOctets = mask.split('.');
  const result = [];

  for (let i = 0; i < 4; i++) {
    result.push(Number(ipOctets[i]) & Number(maskOctets[i]));
  }

  return `${result[0]}.${result[1]}.${result[2]}.${result[3]}`;
}

function getAddressesCount(mask: string): number {
  let binaryMask = '';

  for (const octet of mask.split('.')) {
    binaryMask += Number(octet).toString(2).padStart(8, '0');
  }
  const zeros = 32 - binaryMask.replaceAll('0', '').length;

  if (zeros === 0) return 1;
  if (zeros === 1) return 2;

  return Math.pow(2, zeros) - 2;
}
</script>

<template>
  <form :class="$style.main" @submit.prevent="showResult">
    <div :class="$style.data">
      <div>
        <UiField :label="'IP-адрес'"></UiField>
        <UiInput v-model="ip" :placeholder="'Введите IP (0.0.0.0)'"></UiInput>
      </div>

      <div>
        <UiField :label="'Маска подсети'"></UiField>
        <!-- Используем SUBNET_MASKS как options -->
        <UiSelect v-model="mask" :options="SUBNET_MASKS"></UiSelect>
      </div>
    </div>

    <UiButton type="submit" layout="primary" :disabled="!isIpValid(ip)">Рассчитать</UiButton>

    <div :class="$style.result" v-if="isShowResult && isIpValid(ip)">
      <div>IP: {{ ip }}</div>
      <div>Маска подсети: {{ mask }}</div>
      <div>Адрес сети: {{ getNetworkAdress(ip, mask) }}</div>
      <div>Количество адресов: {{ getAddressesCount(mask) }}</div>
    </div>
  </form>
</template>

<style module>
.main {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 16px;
  margin: auto;
  width: 50%;
  padding-bottom: 50%;
  flex-wrap: wrap;
  
}

.data {
  display: flex;
  flex-direction: column;
  gap: 10px;
}

.result {
  margin-top: 16px;
  padding: 20px;
  background-color: var(--color-back-header);
  border: 1px solid var(--color-border);
  border-radius: 8px;
  color: var(--color-text);
  font-size: 0.875rem;
  line-height: 1.5;
  
}

</style>