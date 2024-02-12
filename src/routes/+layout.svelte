<script lang="ts">
  import { onMount } from 'svelte';
  import { fauth } from "../firebase";
  import "../app.css";
  import { onAuthStateChanged, signOut } from 'firebase/auth';
  import { userStore } from "../stores/userStore";
  import toast, { Toaster } from 'svelte-french-toast';

  let me;

  onMount(() => {
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
      {#if userStore.loggedIn}
        <div tabindex="0" role="button" class="btn btn-ghost btn-circle avatar">
          <div class="w-10 rounded-full">
            <img src={me.photoURL} alt="Rounded avatar" />
          </div>
          <button on:click={logOut}>Sign out</button>
        </div>
      {:else}
        <button class="btn" on:click={() => window.location.href = '/login'}>Login</button>
      {/if}
    </div>
  </div>
</div>

<slot />
