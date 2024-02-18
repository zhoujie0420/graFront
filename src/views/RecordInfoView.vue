<template>
  <div>
    <h2>{{ record.title }}</h2>
    <p>{{ record.content }}</p>
  </div>
</template>

<script>
const recordId = $route.params.id;
console.log(recordId);
</script>
