<script setup>
import { useMouse } from '../use/mouse';
const { x, y } = useMouse();
</script>

<template>
  Mouse position is at: {{ x }}, {{ y }}
</template>
