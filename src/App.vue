<script setup lang="ts">
import { ref } from 'vue';
import Clocky from './components/Clocky.vue';

const localtz = Intl.DateTimeFormat().resolvedOptions().timeZone;
const tzAvaliables = ref((Intl as any).supportedValuesOf('timeZone'));

const newTz = ref<string>()
const list = ref<Array<string>>([localtz])

function add(): void {
  if (newTz.value) {
    list.value.push(newTz.value);
    newTz.value = undefined;
  }
}
function rm(index: number): void {
  list.value.splice(index, 1);
}

</script>

<template>
  <div>
    <div>
      <form action="#">
        <h1>Clocky</h1>
        <p>Select the desire timezone to compare</p>
        <input type="search" id="timezone" list="timezone-list" placeholder="Timezone" v-model="newTz" />
        <datalist id="timezone-list">
          <option v-for="(item, index) in tzAvaliables" :value="item" :key="index">{{ item }}</option>
        </datalist>
        <button class="btn-add" v-on:click="add">Add</button>
      </form>
    </div>
    <div class="container">
      <Clocky v-for="(item, index) in list" :time-zone="item" :key="index" @close="rm(index)"></Clocky>
    </div>
  </div>
</template>

<style scoped>
input#timezone{
  border-radius: 10px 0px 0px 10px;
}
.btn-add{
  border-radius: 0 5px 5px 0;
}

</style>