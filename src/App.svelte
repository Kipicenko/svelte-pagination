<script>
    import Post from './lib/Post.svelte'
    import Pagination from './lib/Pagination/Pagination.svelte'
    import { onMount } from 'svelte';
    let posts = []
    let totalCount = 0
    let currentPage = 1
    let pageSize = 5
    onMount(async () => {
        let res = await fetch('https://jsonplaceholder.typicode.com/posts')
        let allPosts = await res.json()
        totalCount = allPosts.length
    })

    const getPosts = async (currentPage, pageSize) => {
        let res = await fetch(`https://jsonplaceholder.typicode.com/posts?_limit=${pageSize}&_page=${currentPage}`)
        posts = await res.json()
    }
    $: getPosts(currentPage, pageSize)
</script>

<main class="main">
    <div class="main__container">
       <div class="main__center">
           <div class="main__content">
               <h1 class="main__title">Posts</h1>
               {#each posts as post}
                   <Post {post}/>
               {:else}
                   <p class="main__loading">Loading...</p>
               {/each}
               <Pagination
                       {currentPage}
                       {pageSize}
                       {totalCount}
                       on:change={(e) => currentPage = e.detail}
               />
           </div>
       </div>
    </div>
</main>

<style lang="scss">
  :root {
    font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen,
      Ubuntu, Cantarell, 'Open Sans', 'Helvetica Neue', sans-serif;
  }

  :global(html) {
    box-sizing: border-box;
    height: 100%;
  }

  :global(*,
  *::before,
  *::after) {
    box-sizing: inherit;
  }

  :global(body) {
    font-family: 'Open Sans', sans-serif;
    margin: 0;
    height: 100%;
  }

  :global(h1, h2, h3, h4, h5, h6) {
    margin: 0;
    padding: 0;
  }

  :global(p) {
    margin: 0;
    padding: 0;
  }

  :global(ul, ol) {
    margin: 0;
    padding: 0;
    list-style: none;
  }

  :global(a) {
    color: inherit;
    text-decoration: none;
  }

  :global(img) {
    max-width: 100%;
    height: auto;
  }

  :global(figure) {
    margin: 0;
  }

  :global(#app) {
    height: 100%;
  }

  main {
    width: 100%;
    height: 100%;
    padding-bottom: 100px;
  }

  .main__container {
    max-width: 1440px;
    padding: 0 20px;
    margin: 0 auto;
    height: 100%;
  }

  .main__center {
    display: flex;
    justify-content: center;
  }

  .main__title {
    text-align: center;
    padding: 20px 0;
  }

  .main__loading {
    text-align: center;
    margin-bottom: 20px;
  }
</style>
