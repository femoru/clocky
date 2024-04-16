<script setup lang="ts">
import { reactive, ref } from 'vue';
import Clocky from './components/Clocky.vue';

const tz = Intl.DateTimeFormat().resolvedOptions().timeZone;
const newTz = ref()

const tzAvaliables = ref((Intl as any).supportedValuesOf('timeZone'));

const list = reactive(['Europe/Spain'])

const add = () => {
  list.push(newTz.value)
  newTz.value = undefined
}

</script>

<template>
  <div>
    <div>
      <form action="#">
        <input type="search" id="timezone" list="timezone-list" placeholder="Timezone" v-model="newTz" />
        <datalist id="timezone-list">
          <option v-for="(item, index) in tzAvaliables" :value="item" :key="index">{{ item }}</option>
        </datalist>
        <button v-on:click="add">Add</button>
      </form>
    </div>
    <div class="container">
      <Clocky :time-zone="tz" />
      <Clocky v-for="(item, index) in list" :time-zone="item" :id="index"></Clocky>

    </div>
  </div>
</template>

<style scoped>
.container {
  display: flex;
  flex-wrap: wrap;
  gap: 20px 30px;
}
</style>
