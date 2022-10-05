<template>
  <div class="mt-10 text-3xl mx-auto max-w-6xl">
    Dashboard index {{ store }}
  </div>
</template>

<script>
export default {
  name: 'Dashboard',
  computed: {
    store() {
      return window?.localStorage?.test ?? ''
    }
  }
}
</script>