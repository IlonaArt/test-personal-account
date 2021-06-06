<template>
  <ul>
    <li v-for='(doc, index) in docs' :key='index'>
      <DocLink
        :id_login='id_login'
        :TK='TK'
        :IMEI='IMEI'
        :doc_name='doc.doc_name'
        :id_document='doc.id_document'
        :doc_type='doc.doc_type'
      >
      </DocLink>
    </li>
  </ul>
</template>

<script>
import DocLink from './DocLink.vue'

export default {
  name: 'DocsList',
  components: {
    DocLink,
  },
  props: {
    docs: Array,
    id_login: Number,
    TK: String,
    IMEI: String,
  },
}

</script>

<style scoped>
ul {
  max-width: 600px;
  margin: 20px auto 20px;
  padding-left: 0;
}
li {
  list-style: none;
  text-align: left;
  padding-top: 20px;
  padding-bottom: 20px;
  border-bottom: 1px solid #dddccd
}
</style>