<script>
  import Icon from '@iconify/svelte';

  export let avatar;
  export let author;
  export let title;
  export let express;
  export let createdAt;
  export let tags;
  export let mins;
  export let read;
  export let commentCount = 0;
</script>

<div class="container">
  <div class="card">
    <div class="card__body">
      {#each tags as tag, index}
        {#if index < 3}
         <code> <span class="tag tag-blue">{tag}</span></code>
        {/if}
      {/each}
      <h4>{title}</h4>
      <p>{@html express}...</p>
    </div>
    <div class="card__footer">
      <div class="user">
        <img src={avatar} alt="user__image" class="w-5 h-5 max-sm:w-3 max-sm:h-3 rounded-full user__image">
        <div class="user__info">
          <h5>{author}</h5>
          <small>{createdAt}</small>
          <small>· {mins} min read ·</small>
          <small>. {read} reads</small>
<small>. {commentCount} comments</small>
        </div>
      </div>
    </div>
  </div>
</div>
