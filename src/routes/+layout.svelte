<script lang="ts">
	import { onMount } from 'svelte';
	import { themes } from '$lib/themes';
 import '../app.css';
	let currentTheme = '';

	onMount(() => {
		if (typeof window !== 'undefined') {
			const theme = window.localStorage.getItem('theme');
			if (theme && themes.includes(theme)) {
				document.documentElement.setAttribute('data-theme', theme);
				currentTheme = theme;
			}
		}
	});

	function setTheme(event: Event) {
		const select = event.target as HTMLSelectElement;
		const theme = select.value;
		if (themes.includes(theme)) {
			const one_year = 60 * 60 * 24 * 365;
			window.localStorage.setItem('theme', theme);
			document.cookie = `theme=${theme}; max-age=${one_year}; path=/; SameSite=Strict;`;
			document.documentElement.setAttribute('data-theme', theme);
			currentTheme = theme;
		}
	}
</script>


<div class="navbar bg-base-100">
  <div class="navbar-start">
    <div class="dropdown">
      <div tabindex="0" role="button" class="btn btn-ghost btn-circle">
        <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5" fill="none" viewBox="0 0 24 24" stroke="currentColor"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M4 6h16M4 12h16M4 18h7" /></svg>
      </div>
      <ul tabindex="0" class="menu menu-sm dropdown-content mt-3 z-[1] p-2 shadow bg-base-100 rounded-box w-52">
        <li><a href="/">Homepage</a></li>
        <li><a href="/blog">Blog</a></li>
        <li><a href="/about">About</a></li>
      </ul>
    </div>
  </div>
  <div class="navbar-center">
    <a class="btn btn-ghost text-xl">TESThub</a>
  </div>
  <div class="navbar-end">
    <button class="btn btn-ghost btn-circle">
      <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5" fill="none" viewBox="0 0 24 24" stroke="currentColor"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M21 21l-6-6m2-5a7 7 0 11-14 0 7 7 0 0114 0z" /></svg>
    </button>
    <button class="btn btn-ghost btn-circle">
      <div class="indicator">
        <select
		data-choose-theme
		class="select select-bordered select-primary mx-auto my-5 w-full max-w-3xl text-xl capitalize"
		on:change={setTheme}
	>
		<option disabled selected>Choose a theme</option>
		{#each themes as theme}
			<option value={theme} class="capitalize">{theme}</option>
		{/each}
	</select>
      </div>
    </button>
  </div>
</div>



<slot />



