<script setup lang="ts">
defineEmits(['close'])
import { ref, onMounted, onUnmounted } from 'vue'

const props = defineProps<{ timeZone: string }>()

const time = ref()
const day = ref()
let intervalId: any

onMounted(() => {
    if (!isValid(props.timeZone)) {
        return;
    }
    intervalId = setInterval(() => {
        day.value = Intl.DateTimeFormat(navigator.language, {
            day: 'numeric',
            month: 'short',
            year: 'numeric',
            timeZoneName: 'shortOffset',
            timeZone: props.timeZone,
        }).format();
        time.value = Intl.DateTimeFormat(navigator.language, {
            hour: '2-digit',
            minute: 'numeric',
            second: 'numeric',
            timeZone: props.timeZone,
        }).format();
    }, 1000);
})


function isValid(tz: string): boolean {
    if (!Intl?.DateTimeFormat().resolvedOptions().timeZone) {
        throw new Error('Time zones are not available in this environment');
    }
    try {
        Intl.DateTimeFormat(undefined, { timeZone: tz });
        return true;
    }
    catch (ex) {
        return false;
    }
}
onUnmounted(() => clearInterval(intervalId))

</script>

<template>
    <div class="notice" v-if="isValid(timeZone)">
        <button class="close-btn" @click="$emit('close')">x</button>
        <p>{{ timeZone }}</p>
        <p>{{ day }}</p>
        <p>{{ time }}</p>
    </div>
    <div v-else class="notice">
        <p>Invalid timezone</p>
    </div>
</template>
<style scoped>
.notice {
    position: relative !important;
    width: 250px;
}

.close-btn {
    position: absolute;
    width: 40px;
    height: 40px;
    top: -2px;
    right: -2px;
}
</style>