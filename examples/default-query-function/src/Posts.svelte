<script>
  import { useQuery, useQueryClient } from '@tanstack/svelte-query'

  export let setPostId

  const client = useQueryClient()
  const posts = useQuery('/posts')
</script>

<div>
  <h1>Posts</h1>
  <div>
    {#if $posts.status === 'loading'}
      <span><span>Loading...</span></span>
    {:else if $posts.status === 'error'}
      <span>Error: {$posts.error.message}</span>
    {:else}
      <div>
        {#each $posts.data as post}
          <p>
            <span
              on:click={() => setPostId(post.id)}
              style={// We can use the queryCache here to show bold links for
              // ones that are cached
              client.getQueryData(`/posts/${post.id}`) ? 'color: green; font-weight: bold; cursor: pointer;' : 'cursor: pointer;'}>
              {post.title}
            </span>
          </p>
        {/each}
      </div>
      <div>{$posts.isFetching ? 'Background Updating...' : ' '}</div>
    {/if}
  </div>
</div>
