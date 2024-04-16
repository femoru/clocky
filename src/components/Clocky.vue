<script setup lang="ts">
import { ref, onMounted, onUnmounted } from 'vue'

const props = defineProps<{ timeZone: string }>()

const time = ref()
const day = ref()
let intervalId: any

onMounted(() => {
    if (!isValid(props.timeZone)) {
        return
    }
    intervalId = setInterval(() => {
        day.value = Intl.DateTimeFormat(navigator.language, {
            day: 'numeric',
            month: 'short',
            year: 'numeric',
            timeZoneName: 'shortOffset',
            timeZone: props.timeZone,
        }).format()
        time.value = Intl.DateTimeFormat(navigator.language, {
            hour: '2-digit',
            minute: 'numeric',
            second: 'numeric',
            timeZone: props.timeZone,
        }).format()
    }, 1000)
})


const isValid = (tz: string): boolean => {
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
        <p>{{ timeZone }}</p>
        <p>{{ day }}</p>
        <p>{{ time }}</p>
    </div>
    <div v-else class="notice">
        <p>Invalid timezone</p>
    </div>
</template>