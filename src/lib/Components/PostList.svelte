<script>
  
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

<!-- svelte-ignore a11y-click-events-have-key-events -->
<div on:click class="font-hind p-2 text-black dark:text-gray-400 variant-glass rounded">
  <div class="text-xl font-bold">{title}</div>
  <div class="text-md exp">{@html express}...</div>
  <div class="flex space-x-1 items-center">
    <img
      class="w-5 h-5 max-sm:w-3 max-sm:h-3 rounded-full"
      src={avatar}
      alt="Rounded avatar"
    />
    
    <div class="text-sm font-bold max-sm:text-[10px]">{author}</div>
    <svg xmlns="http://www.w3.org/2000/svg" width="1em" height="1em" viewBox="0 0 256 256" {...$$props}>
	<path fill="black" d="M128 80a48 48 0 1 0 48 48a48 48 0 0 0-48-48m0 60a12 12 0 1 1 12-12a12 12 0 0 1-12 12" />
</svg> 
<svg xmlns="http://www.w3.org/2000/svg" width="1em" height="1em" viewBox="0 0 24 24" {...$$props}>
	<path fill="black" d="M12 9a3 3 0 0 0-3 3a3 3 0 0 0 3 3a3 3 0 0 0 3-3a3 3 0 0 0-3-3m0 8a5 5 0 0 1-5-5a5 5 0 0 1 5-5a5 5 0 0 1 5 5a5 5 0 0 1-5 5m0-12.5C7 4.5 2.73 7.61 1 12c1.73 4.39 6 7.5 11 7.5s9.27-3.11 11-7.5c-1.73-4.39-6-7.5-11-7.5" />
</svg>{read} <svg xmlns="http://www.w3.org/2000/svg" width="1em" height="1em" viewBox="0 0 256 256" {...$$props}>
	<path fill="black" d="M128 80a48 48 0 1 0 48 48a48 48 0 0 0-48-48m0 60a12 12 0 1 1 12-12a12 12 0 0 1-12 12" />
</svg> <svg xmlns="http://www.w3.org/2000/svg" width="1em" height="1em" viewBox="0 0 16 16" {...$$props}>
	<path fill="black" fill-rule="evenodd" d="m4.843 10.944l-.194 2.335a.204.204 0 0 0 .339.17l2.21-1.964l.589.013L8 11.5c1.695 0 3.087-.44 4.02-1.177c.89-.702 1.48-1.76 1.48-3.323s-.59-2.62-1.48-3.323C11.087 2.94 9.695 2.5 8 2.5c-1.695 0-3.087.44-4.02 1.177C3.09 4.38 2.5 5.437 2.5 7c0 1.648.656 2.742 1.648 3.448zm1.141 3.625l1.77-1.572C7.834 13 7.916 13 8 13c3.866 0 7-2 7-6s-3.134-6-7-6s-7 2-7 6c0 2.117.878 3.674 2.277 4.67l-.123 1.484a1.704 1.704 0 0 0 2.83 1.415" clip-rule="evenodd" />
</svg> {commentCount} </div>
  </div>
  <div
    class="flex flex-wrap gap-2 mt-2 items-center text-gray-500 font-poppins text-sm max-sm:text-[10px]"
  >
    <div>{createdAt}</div>
    · {mins} min read ·
    {#each tags as tag, index}
      {#if index<3}
      <code>{tag}</code>
      {/if}
    {/each}
  </div>
</div>
<hr />

<style>
  
  .read{
    align-items: center;
  }
</style>
