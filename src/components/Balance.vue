<template>
<div>
    <h4 id="balance-box">Your Balance <br>$ {{ total }}</h4>
</div>
</template>

<script setup>
import { defineProps } from 'vue';

const props = defineProps({
    total: {
        type: Number,
        required: true,
    }
});
</script>