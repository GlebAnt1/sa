<script setup lang="ts">
  import { ref } from 'vue'
  import { MASK_OPTIONS } from '../constants/maskOptions'
  import { isIpValid } from '../utils/ipValidation'
  import { getNetworkAdress } from '../utils/networkAddress'
  import { getAddressesCount } from '../utils/addressesCount'

  const ip = ref('')
  const mask = ref('255.255.255.255')
  const isShowResult = ref(false)

  function showResult() {
    isShowResult.value = true
  }
</script>

<template>
  <div class="calculator">
    <form class="form" @submit.prevent="showResult">
      <h1 class="title">КАЛЬКУЛЯТОР ПОДСЕТЕЙ</h1>

  <div class="input-group">
    <label class="label">IP адрес:</label>
    <input v-model="ip" class="input" placeholder="192.168.1.1">
  </div>

  <div class="input-group">
    <label class="label">Маска подсети:</label>
    <select v-model="mask" class="select">
      <option
        v-for="option in MASK_OPTIONS"
        :key="option"
        :value="option"
      >
        {{ option }}
      </option>
    </select>
  </div>

      <button class="button" type="submit" :disabled="!isIpValid(ip)">
        Рассчитать
      </button>
    </form>

    <div class="result" v-if="isShowResult && isIpValid(ip)">
      <h2 class="result-title">Результат:</h2>
      <div class="result-item">IP: {{ ip }}</div>
      <div class="result-item">Маска подсети: {{ mask }}</div>
      <div class="result-item">Адрес сети: {{ getNetworkAdress(ip, mask) }}</div>
      <div class="result-item">Количество адресов: {{ getAddressesCount(mask) }}</div>
    </div>
  </div>
</template>

<style scoped>
  .calculator {
    max-width: 600px;
    margin: 0 auto;
    padding: 20px;
    font-family: 'base', sans-serif;
  }

  .form {
    display: flex;
    flex-direction: column;
    gap: 20px;
    padding: 20px;
    border: 1px solid var(--color-gray);
    border-radius: 8px;
    background-color: var(--color-white);
  }

  .title {
    text-align: center;
    color: var(--color-primary);
    margin-bottom: 0;
  }

  .input-group {
    display: flex;
    flex-direction: column;
    gap: 8px;
  }

  .label {
    font-weight: 700;
    color: var(--color-black);
  }

  .input,
  .select {
    padding: 10px;
    border: 1px solid var(--color-gray);
    border-radius: 4px;
    font-size: 1rem;
  }

  .input:focus,
  .select:focus {
    outline: none;
    border-color: var(--color-primary);
  }

  .button {
    padding: 12px;
    background-color: var(--color-primary);
    color: var(--color-white);
    border: none;
    border-radius: 4px;
    font-size: 1rem;
    font-weight: 700;
    cursor: pointer;
    transition: background-color 0.3s;
  }

  .button:hover:not(:disabled) {
    background-color: #0056a3;
  }

  .button:disabled {
    background-color: var(--color-gray);
    cursor: not-allowed;
  }

  .result {
    margin-top: 20px;
    padding: 20px;
    border: 1px solid var(--color-gray);
    border-radius: 8px;
    background-color: var(--color-white);
  }

  .result-title {
    color: var(--color-primary);
    margin-bottom: 16px;
  }

  .result-item {
    margin-bottom: 8px;
    color: var(--color-black);
  }
</style>
