<script>
  import { onMount } from 'svelte';
  import { onAuthStateChanged, signOut } from 'firebase/auth';
  import { fauth } from "../firebase";
  import { userStore } from "../stores/userStore";
  import { themes } from '$lib/themes';
  import "../app.css";
  let me;

  onMount(async () => {
    document.title = "TESTHub | Home";
    const theme = window.localStorage.getItem('theme');
    if (theme && themes.includes(theme)) {
      document.documentElement.setAttribute('data-theme', theme);
    }

    onAuthStateChanged(fauth, async (user) => {
      if (user) {
        userStore.set({ ...user, loggedIn: true });
        me = user;
      } else {
        me = false;
        console.log('User Not Logged In!');
        userStore.set({ loggedIn: false });
      }
    });
  });

  const logOut = async () => {
    try {
      await signOut(fauth);
    } catch (err) {
      console.log(err);
    }
  }

  function setTheme(event) {
    const select = event.target;
    const theme = select.value;
    if (themes.includes(theme)) {
      const one_year = 60 * 60 * 24 * 365;
      window.localStorage.setItem('theme', theme);
      document.cookie = `theme=${theme}; max-age=${one_year}; path=/; SameSite=Strict;`;
      document.documentElement.setAttribute('data-theme', theme);
    }
  }
</script>

<div class="bg-base-100">
  <div class="navbar max-w-screen-xl mx-auto">
    <div class="navbar-start">
      <div class="dropdown">
        <div tabindex="0" role="button" class="btn btn-ghost lg:hidden">
          <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5" stroke="currentColor" class="w-6 h-6">
            <path stroke-linecap="round" stroke-linejoin="round" d="M3.75 9h16.5m-16.5 6.75h16.5"/>
          </svg>
        </div>
        <ul tabindex="0" class="menu menu-sm dropdown-content mt-3 z-[1] p-2 shadow bg-base-100 rounded-box w-52">
          <li><a href="/" class="/">Home</a></li>
          <li><a href="/about" class="/about">About</a></li>
          <li><a href="/projects" class="/projects">Projects</a></li>
          <li><a href="/arts" class="/arts">Arts</a></li>
        </ul>
      </div>
    </div>
    <h1 class="btn btn-ghost text-xl center"></h1>
    <div class="navbar-center hidden lg:flex">
      <ul class="menu menu-horizontal px-1">
        <li><a href="/" class="/">Home</a></li>
        <li><a href="/about" class="/about">About</a></li>
        <li><a href="/projects" class="/projects">Projects</a></li>
        <li><a href="/arts" class="/arts">Arts</a></li>
      </ul>
    </div>
    <div class="navbar-end">
      {#if me}
      <div tabindex="0" role="button" class="btn btn-ghost btn-circle avatar">
        <div class="w-10 rounded-full">
          <img src={me.photoURL} alt="Rounded avatar" />
        </div>
        <button on:click={logOut}>Sign out</button>
      </div>
      {:else}
      <button class="btn" on:click={() => window.location.href = '/auth'}><svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5" stroke="currentColor" class="w-5 h-5">
          <path stroke-linecap="round" stroke-linejoin="round" d="M9.813 15.904 9 18.75l-.813-2.846a4.5 4.5 0 0 0-3.09-3.09L2.25 12l2.846-.813a4.5 4.5 0 0 0 3.09-3.09L9 5.25l.813 2.846a4.5 4.5 0 0 0 3.09 3.09L15.75 12l-2.846.813a4.5 4.5 0 0 0-3.09 3.09ZM18.259 8.715 18 9.75l-.259-1.035a3.375 3.375 0 0 0-2.455-2.456L14.25 6l1.036-.259a3.375 3.375 0 0 0 2.455-2.456L18 2.25l.259 1.035a3.375 3.375 0 0 0 2.456 2.456L21.75 6l-1.035.259a3.375 3.375 0 0 0-2.456 2.456ZM16.894 20.567 16.5 21.75l-.394-1.183a2.25 2.25 0 0 0-1.423-1.423L13.5 18.75l1.183-.394a2.25 2.25 0 0 0 1.423-1.423l.394-1.183.394 1.183a2.25 2.25 0 0 0 1.423 1.423l1.183.394-1.183.394a2.25 2.25 0 0 0-1.423 1.423Z"/>
        </svg>
        Get in touch</button>
      {/if}
    </div>
  </div>
</div>

<slot />

<footer class="bg-base-200">
  <div class="footer text-base-content border-base-300 flex px-10 py-4 border-t max-w-screen-xl mx-auto">
    <aside class="items-center grid-flow-col mt-2">
      <p>© 2024 <b>TESTHub</b></p>
    </aside>
    <div class="dropdown dropdown-top dropdown-end place-self-center justify-self-end ml-auto">
      <div tabindex="0" role="button" class="btn btn-sm bg-base-100">Theme
        <svg width="12px" height="12px" class="h-2 w-2 fill-current opacity-60 inline-block" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 2048 2048">
          <path d="M1799 349l242 241-1017 1017L7 590l242-241 775 775 775-775z"/>
        </svg>
      </div>
      <ul tabindex="0" class="dropdown-content z-[1] p-2 shadow-2xl bg-base-300 rounded-box w-52">
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
      </ul>
    </div>
  </div>
</footer>
