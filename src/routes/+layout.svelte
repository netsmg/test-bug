<script lang="ts">
  import { onMount } from 'svelte';
  import { themes } from '$lib/themes';
  import { fauth } from "../firebase";
  import { onAuthStateChanged, signOut } from 'firebase/auth';
  import { userStore } from "../stores/userStore";
  import toast, { Toaster } from 'svelte-french-toast';

  let me;

  onMount(async () => {
    document.title = "TESThub | Home";
    
    const saveSettings = () => {
      return new Promise((resolve, reject) => {
        let x = setInterval(() => {
          if (userStore.loggedIn === true) {
            resolve();
            clearInterval(x);
          } else if (userStore.loggedIn === false) {
            reject();
            clearInterval(x);
          }
        }, 100)
      });
    }

    toast.promise(
      saveSettings(),
      {
        loading: 'Checking...',
        success: 'Signed In!',
        error: 'You are not signed In!',
      },
    );

    onAuthStateChanged(fauth, async (user) => {
      if (user) {
        userStore.set({...user, loggedIn: true});
        me = user;
      } else {
        me = false;
        console.log('User Not Logged In!');
        userStore.set({loggedIn: false});
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

  function setTheme(event: Event) {
    const select = event.target as HTMLSelectElement;
    const theme = select.value;
    if (themes.includes(theme)) {
      const one_year = 60 * 60 * 24 * 365;
      window.localStorage.setItem('theme', theme);
      document.cookie = `theme=${theme}; max-age=${one_year}; path=/; SameSite=Strict;`;
      document.documentElement.setAttribute('data-theme', theme);
    }
  }
</script>

<Toaster />
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
  <div class="navbar-start">
    <a class="btn btn-ghost text-xl">TESThub</a>
  </div>
  <div class="dropdown dropdown-end">
    {#if me}
    <div tabindex="0" role="button" class="btn btn-ghost btn-circle avatar">
      <div class="w-10 rounded-full">
        <img src={me.photoURL} alt="Rounded avatar" />
      </div>
    </div>
    <ul tabindex="0" class="mt-3 z-[1] p-2 shadow menu menu-sm dropdown-content bg-base-100 rounded-box w-52">
      <li>
        <a class="justify-between">
          Profile
          <span class="badge">New</span>
        </a>
      </li>
      <li><a>Settings</a></li>
      <li on:click={logOut}>Logout</li>
    </ul>
</div>
    {:else}
    <div><a href="/auth">Login</a></div>
  {/if}
  <div class="navbar-end">
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


<slot />
