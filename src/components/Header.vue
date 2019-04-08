<template lang="html">
  <div class="header-container">
    <b-nav tabs>
      <b-nav-item disabled>Quiz</b-nav-item>
      <b-nav-item disabled>{{ numCorrect }} / {{ numTotal }}</b-nav-item>
    </b-nav>
  </div>
</template>

<script>
export default {
  props: [
    'numTotal',
    'numCorrect'
  ]
}
</script>

<style lang="css" scoped>
</style>
