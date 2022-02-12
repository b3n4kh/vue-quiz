<template>
  <div>
    <b-nav tabs>
    <select :catalog="catalog" @change="catalogChange">
      <option value="src">SeeFunk</option>
      <option value="src_tk">Technik</option>
      <option value="src_re">Recht</option>
      <option value="src_sb">SonderBestimmungen</option>
    </select>
      <b-nav-item disabled>Correct: {{ numCorrect }} Answered: {{ numAnswered }} Total: {{ numTotal }}</b-nav-item>
    </b-nav>
  </div>
</template>

<script>
export default {
  props: [
    'numCorrect',
    'numTotal',
    'numAnswered',
    'catalog'
  ],
  methods: {
    catalogChange(event) {
      this.$emit('catalogChanged', event.target.value)
    }
  }
}
</script>
