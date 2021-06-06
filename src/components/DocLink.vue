<template>
  <a :href='docUrl' download title='Скачать'>
    <img width='48px' height='48px' src='../assets/pdf.svg' alt=''>
    {{ doc_name }}
  </a>
</template>

<script>

export default {
  name: 'DocLink',
  data() {
    return {
      docUrl: '',
    }
  },
  props: {
    doc_name: String,
    doc_type: Number,
    id_document: Number,
    id_login: Number,
    TK: String,
    IMEI: String,
  },
  async created() {
    const params = JSON.stringify({
      id_login: this.id_login,
      id_people: this.id_login,
      TK: this.TK,
      IMEI: this.IMEI,
      Name_app: "connect",
      Name_event: "get_pic_path",
      id_document: this.id_document,
      doc_type: this.doc_type,
    })
    const result = await fetch(`https://host1.medsafe.tech:40443/api/test?json=${params}`)
    const downloadData = await result.json();
    this.docUrl = `https://host1.medsafe.tech:40443/${downloadData.body[0].hash}`;
  },
}
</script>

<style scoped>
a {
  display: block;
  cursor: pointer;
}
img {
  vertical-align: middle;
}
</style>