<template>
  <div>Home page</div>
  <div>{{ user }}</div>
</template>

<script lang="ts" setup>
  const user = useCurrentUser();
</script>
