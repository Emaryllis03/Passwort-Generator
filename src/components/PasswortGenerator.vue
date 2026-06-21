<script setup lang="ts">
import { ref } from 'vue'

const uppercase = ref(false)
const lowercase = ref(false)
const SpecialChars = ref(false)
const NumbContain = ref(false)
const length = ref(8)
const password = ref('')
const copied = ref(false)

function generatePassword() {
  const upper = 'ABCDEFGHIJKLMNOPQRSTUVWXYZ'
  const lower = 'abcdefghijklmnopqrstuvwxyz'
  const numbers = '0123456789'
  const special = '!@#$%^&*()_+-=[]{}|;:,.<>?/'

  let allChars = ''
  const requiredChars: string[] = []

  if (uppercase.value) {
    allChars += upper
    requiredChars.push(upper[Math.floor(Math.random() * upper.length)]!)
  }

  if (lowercase.value) {
    allChars += lower
    requiredChars.push(lower[Math.floor(Math.random() * lower.length)]!)
  }

  if (NumbContain.value) {
    allChars += numbers
    requiredChars.push(numbers[Math.floor(Math.random() * numbers.length)]!)
  }

  if (SpecialChars.value) {
    allChars += special
    requiredChars.push(special[Math.floor(Math.random() * special.length)]!)
  }

  if (allChars.length === 0) {
    password.value = 'Bitte mindestens 1 Option wählen'
    return
  }

  let result = [...requiredChars]

  for (let i = result.length; i < length.value; i++) {
    const randomIndex = Math.floor(Math.random() * allChars.length)
    result.push(allChars[randomIndex]!)
  }

  result = result.sort(() => Math.random() - 0.5)

  password.value = result.join('')
}

function copyPassword() {
  if (!password.value) return

  navigator.clipboard.writeText(password.value)

  copied.value = true

  setTimeout(() => {
    copied.value = false
  }, 1500)
}
</script>

<template>
  <div class="row">
    <input type="checkbox" id="Großbuchstaben" name="Großbuchstaben" v-model="uppercase" />
    <label for="Großbuchstaben">Großbuchstaben</label>
  </div>

  <div class="row">
    <input type="checkbox" id="Kleinbuchstaben" name="Kleinbuchstaben" v-model="lowercase" />
    <label for="Kleinbuchstaben">Kleinbuchstaben</label>
  </div>

  <div class="row">
    <input type="checkbox" id="Sonderbuchstaben" name="Sonderbuchstaben" v-model="SpecialChars" />
    <label for="Sonderbuchstaben">Sonderbuchstaben</label>
  </div>

  <div class="row">
    <input type="number" id="Länge" name="Länge" v-model.number="length" />
    <label for="Länge">Länge</label>
  </div>

  <div class="row">
    <input type="checkbox" id="Zahlen" name="Zahlen" v-model="NumbContain" />
    <label for="Zahlen">Zahlen</label>
  </div>

  <button @click="generatePassword()">Generier Passwort</button>

  {{ password }}
  <button @click="copyPassword">Kopieren</button>
  <p v-if="copied">✔ Kopiert!</p>
</template>

<style scoped></style>
