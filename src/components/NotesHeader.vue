<template>
  <div>
    <h2>{{ title }}</h2>
    <h4> Use doubleclick on chips to trigger edit action </h4>
  </div>
</template>

<script>
export default {
  name: 'NotesHeader',
  props: {
    title: {
      default: 'Sanek Compoments Enterprises'
    }
  }
};
</script>

<style lang="scss" scoped>
  h2 {
    text-align: center;
    font-weight: 500;
    color: grey;
    text-shadow: 0 2px 3px rgba(255, 255, 255, 0.5);
  }
  h4 {
    text-align: center;
    font-weight: 300;
    color: grey;
    text-shadow: 0 2px 3px rgba(255, 255, 255, 0.5);
  }
</style>

