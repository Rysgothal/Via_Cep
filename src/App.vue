<script setup>
import { ref } from 'vue'
import axios from 'axios'

const cep = ref('')
const logradouro = ref('')
const cidade = ref('')
const uf = ref('')
const numero = ref('')
const isFormValid = ref(false)

const sucessMessage = async () => {
  alert('Endereço cadastrado com sucesso!');
}

const fetchAddress = async () => {
  console.log('Buscando informações...')

  if (cep.value.length === 8) {
    try {
      const response = await axios.get(`https://viacep.com.br/ws/${cep.value}/json/`)
      const data = response.data
      logradouro.value = data.logradouro
      cidade.value = data.localidade
      uf.value = data.uf
      validateForm()
    } catch (error) {
      console.error('Inconsistência ao buscar informações:', error)
    }
  } else {
      logradouro.value = ''
      cidade.value = ''
      uf.value = ''
      numero.value = ''
  }
}

const validateForm = () => {
  isFormValid.value = cep.value && logradouro.value && cidade.value && uf.value && numero.value
}

</script>

<template>
  <div>
    <div style="background-color: antiquewhite; padding: 1px; border-radius: 10px;">
        <div style="background-color: black; border-radius: 10px; padding: 15px;">
          <h1 style="text-align: center; color: beige; font-size: xxx-large; font-weight: bold;">Via CEP</h1>
          <a class="Green" href="https://viacep.com.br" target="_blank" rel="noopener">Via CEP - Consulte CEPs de todo o Brasil</a>
        </div>
    </div>

    <div style="padding: 10px;"></div>

    <form @submit.prevent="validateForm">
      <div style="padding: 20px; border-style: dashed; border-width: 1px; border-color: cornflowerblue;">
        <div style="flex-direction: column; align-items: center;">
          <div style="margin-bottom: 10px; display: flex; justify-content: space-between; width: 100%; max-width: 400px;">
            <label for="cep" style="flex: 1; text-align: left;">CEP:</label>
            <input id="cep" v-model="cep" maxlength="8" style="flex: 2px;" @blur="fetchAddress" @input="fetchAddress"/>
          </div>

          <div style="margin-bottom: 10px; display: flex; justify-content: space-between; width: 100%; max-width: 400px;">
            <label for="logradouro" style="flex: 1; text-align: left;">Logradouro:</label>
            <input id="logradouro" v-model="logradouro" readonly required style="flex: 2px;" />
          </div>

          <div style="margin-bottom: 10px; display: flex; justify-content: space-between; width: 100%; max-width: 400px;">
            <label for="cidade" style="flex: 1; text-align: left;">Cidade:</label>
            <input id="cidade" v-model="cidade" readonly required style="flex: 2px;" />
          </div>

          <div style="margin-bottom: 10px; display: flex; justify-content: space-between; width: 100%; max-width: 400px;">
            <label for="uf" style="flex: 1; text-align: left;">UF:</label>
            <input id="uf" v-model="uf" readonly required style="flex: 2px;" />
          </div>

          <div style="margin-bottom: 10px; display: flex; justify-content: space-between; width: 100%; max-width: 400px;">
            <label for="numero" style="flex: 1; text-align: left;">Número:</label>
            <input id="numero" v-model="numero" @input="validateForm" required style="flex: 2px;" />
          </div>
        </div>
        <div style="display: flex; justify-content: flex-end; align-items: center;">
            <button type="submit" :disabled="!isFormValid" v-on:click="sucessMessage">Concluir</button>
        </div>       
      </div>
    </form>
  </div>
</template>
