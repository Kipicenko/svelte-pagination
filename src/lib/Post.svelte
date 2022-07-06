<script>
  export let post;
</script>

<article class="post">
    <div class="container__post">
        <div class="post__id">
            id: {post.id}
        </div>
        <h2 class="post__title">
            <span class="post__name">Title:</span> {post.title}
        </h2>
        <p class="post__description">
            <span class="post__name">Description:</span> {post.body}
        </p>
    </div>
</article>

<style lang="scss">
    .post:not(:last-child) {
      margin-bottom: 20px;
    }

    .post {
      background-color: #E5E1E8;
      border-radius: 4px;
      padding: 10px;
      max-width: 800px;

      &__id {
        margin-bottom: 10px;
      }

      &__title {
        font-size: 16px;
        margin-bottom: 10px;
      }

      &__name {
        font-weight: 400;
        font-size: 18px;
        color: #EB8A54;
      }
    }
</style>