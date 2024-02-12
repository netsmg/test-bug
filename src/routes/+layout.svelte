<script>
  import { onMount } from 'svelte';
  import { onAuthStateChanged, signOut } from 'firebase/auth';
  import { fauth } from "../firebase";
  import { userStore } from "../stores/userStore";
  import toast, { Toaster } from 'svelte-french-toast';
  import { themes } from '$lib/themes';
  import "../app.css";
  let me;

  onMount(async () => {
    document.title = "TESTHub | Home";
    const theme = window.localStorage.getItem('theme');
    if (theme && themes.includes(theme)) {
      document.documentElement.setAttribute('data-theme', theme);
    }

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

<svelte:head>
  <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/animate.css/4.1.1/animate.min.css"/>
  <script src="//cdnjs.cloudflare.com/ajax/libs/highlight.js/11.7.0/highlight.min.js"></script>
  <link rel="stylesheet" href="https://firebasestorage.googleapis.com/v0/b/blog-424dc.appspot.com/o/dracula.css?alt=media&token=7a146a01-151e-404a-87d4-b4fce940de8e"/>
  <script src="//cdnjs.cloudflare.com/ajax/libs/highlightjs-line-numbers.js/2.8.0/highlightjs-line-numbers.min.js"></script>
  <script src="https://polyfill.io/v3/polyfill.min.js?features=es6"></script>
  <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css"/>
</svelte:head>

<Toaster/>

<div class="navbar bg-base-100">
  <div class="navbar-start">
    <div class="dropdown">
      <div tabindex="0" role="button" class="btn btn-ghost btn-circle">
        <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5" fill="none" viewBox="0 0 24 24" stroke="currentColor"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M4 6h16M4 12h16M4 18h7" /></svg>
      </div>
      <ul tabindex="0" class="menu menu-sm dropdown-content mt-3 z-[1] p-2 shadow bg-base-100 rounded-box w-52">
        <li><a href="/">Homepage</a></li>
        <li><a>Portfolio</a></li>
        <li><a>About</a></li>
      </ul>
    </div>
  </div>
  
  <div class="flex-1">
    <a class="btn btn-ghost text-xl">TESThub</a>
  </div>
  
  <div class="flex-none gap-2">
    <div class="form-control">
      <input type="text" placeholder="Search" class="input input-bordered w-24 md:w-auto" />
    </div>
    <div class="dropdown dropdown-end">
      {#if me}
        <div tabindex="0" role="button" class="btn btn-ghost btn-circle avatar">
          <div class="w-10 rounded-full">
            <img src={me.photoURL} alt="Rounded avatar" />
          </div>
          <button on:click={logOut}>Sign out</button>
        </div>
      {:else}
        <button class="btn" on:click={() => window.location.href = '/auth'}>Login</button>
      {/if}
    </div>
  </div>
</div>

<div class="mx-auto w-full max-w-xl px-10">
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
