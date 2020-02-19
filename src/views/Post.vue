<template>
  <div class="wrapper">
    <prismic-edit-button :documentId="documentId" />
    <h1 class="title">{{ $prismic.richTextAsPlain(fields.title) }}</h1>
  </div>
</template>

<script>
export default {
  name: 'Post',
  data() {
    return {
      documentId: '',
      fields: {
        title: null,
      },
    };
  },
  methods: {
    getContent(uid) {
      this.$prismic.client.getByUID('page', uid).then(document => {
        if (document) {
          this.documentId = document.id;
          this.fields.title = document.data.title;
        } else {
          this.$router.push({ name: 'not-found' });
        }
      });
    },
  },
  created() {
    this.getContent(this.$route.params.uid);
  },
  beforeRouteUpdate(to, from, next) {
    this.getContent(to.params.uid);
    next();
  },
};
</script>

<style></style>
