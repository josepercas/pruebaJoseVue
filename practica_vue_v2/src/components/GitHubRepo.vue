<template>
<div v-for="dato in datos"><a :href="dato.html_url" target="#" :title="dato.description"> {{ dato.full_name}} </a> Forks: {{ dato.forks_count }}</div>

</template>
  
<script>

export default {
  name: 'GitHubRepo',
  props: ['datos'],
  data() {
    return {
      datosUsar: this.datos,
    }
  },
  methods: {
  }
}



</script>



<!-- Add "scoped" attribute to limit CSS to this component only -->
<style>
</style>
  