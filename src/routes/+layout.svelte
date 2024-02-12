<script lang="ts">
  import { onMount } from 'svelte';
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
</script>

<Toaster />

<div class="navbar bg-base-100">
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
        <ul tabindex="0" class="mt-3 z-[1] p-2 shadow menu menu-sm dropdown-content bg-base-100 rounded-box w-52">
          <li><a href="/">Homepage</a></li>
          <li><a href="/blog">Blog</a></li>
          <li><a href="/about">About</a></li>
          <li><a href="/login">Login</a></li>
        </ul>
      {/if}
    </div>
  </div>
</div>

<slot />
