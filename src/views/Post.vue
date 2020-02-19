<template>
  <div class="wrapper">
    <prismic-edit-button :documentId="documentId" />
    <h1 class="title">{{ $prismic.richTextAsPlain(fields.title) }}</h1>
    <h2 class="subtitle">{{ $prismic.richTextAsPlain(fields.subtitle) }}</h2>
    <span class="date">Posted on: {{ fields.postdate }}</span>
    <prismic-rich-text :field="fields.content" class="content" />
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
      this.$prismic.client.getByUID('post', uid).then(document => {
        if (document) {
          this.documentId = document.id;
          this.fields.title = document.data.title;
          this.fields.subtitle = document.data.subtitle;
          this.fields.postdate = document.data.postdate;
          this.fields.content = document.data.content;
        } else {
          this.$router.push({ name: 'not-found' });
        }
      });
    },
  },
  created() {
    console.log('UID:', this.$route.params.uid);
    this.getContent(this.$route.params.uid);
  },
  beforeRouteUpdate(to, from, next) {
    this.getContent(to.params.uid);
    next();
  },
};
</script>

<style>
p {
  margin: 15px 0;
}

p.block-img {
  text-align: center;
}

ul {
  margin-left: 30px;
}

.wrapper {
  max-width: 820px;
  margin-left: auto;
  margin-right: auto;
  padding: 40px 10px;
  font-family: Avenir, 'Helvetica Neue', Helvetica, Arial, sans-serif;
}

.title {
  font-size: 1.5rem;
  color: #333;
}

.subtitle {
  font-size: 1.2rem;
  color: #666;
}

.date {
  font-size: 0.8rem;
  color: #999;
  font-style: italic;
}

.content {
  margin: 30px 0;
}
</style>
