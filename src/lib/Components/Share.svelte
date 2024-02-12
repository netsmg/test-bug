<script>
  import { onMount } from "svelte";

  let currentPageLink = "";

  onMount(() => {
    // Assuming you want the current page link
    currentPageLink = window.location.href;
  });

  function copyLink() {
    const textField = document.createElement("textarea");
    textField.innerText = currentPageLink;
    document.body.appendChild(textField);
    textField.select();
    document.execCommand("copy");
    textField.remove();
    alert("Link copied to clipboard!");
  }
</script>

<style>
  /* Add your styles here */
</style>

<div class="flex items-center justify-center">
  <div class="bg-gray-100 w-full mx-4 p-4 rounded-xl md:w-1/2 lg:w-1/3">
    <!-- MODAL HEADER -->
    <div class="flex justify-between items-center border-b border-gray-200 py-3">
      <div
        class="bg-gray-300 hover:bg-gray-500 cursor-pointer hover:text-gray-300 font-sans text-gray-500 w-8 h-8 flex items-center justify-center rounded-full"
      >
        
      </div>
    </div>

    <!-- MODAL BODY -->
    <div class="my-4">
      <p class="text-sm">Share this link via</p>

      <!-- SOCIAL MEDIA ICONS -->
      <div class="flex justify-around my-4">
        <!-- Add or modify social media icons as needed -->

        <!-- CUSTOM ICON FOR CURRENT PAGE LINK -->
        <div
          class="border hover:bg-gray-500 w-12 h-12 fill-gray-500 hover:fill-white border-gray-200 rounded-full flex items-center justify-center shadow-xl hover:shadow-blue-500/50 cursor-pointer"
          on:click={copyLink}
        >
          <svg
            xmlns="http://www.w3.org/2000/svg"
            width="24"
            height="24"
            viewBox="0 0 24 24"
            class="fill-current"
          >
            <path
              d="M8.465 11.293c1.133-1.133 3.109-1.133 4.242 0l.707.707 1.414-1.414-.707-.707c-.943-.944-2.199-1.465-3.535-1.465s-2.592.521-3.535 1.465L4.929 12a5.008 5.008 0 0 0 0 7.071 4.983 4.983 0 0 0 3.535 1.462A4.982 4.982 0 0 0 12 19.071l.707-.707-1.414-1.414-.707.707a3.007 3.007 0 0 1-4.243 0 3.005 3.005 0 0 1 0-4.243l2.122-2.121z"
            ></path>
            <path
              d="m12 4.929-.707.707 1.414 1.414.707-.707a3.007 3.007 0 0 1 4.243 0 3.005 3.005 0 0 1 0 4.243l-2.122 2.121c-1.133 1.133-3.109 1.133-4.242 0L10.586 12l-1.414 1.414.707.707c.943.944 2.199 1.465 3.535 1.465s2.592-.521 3.535-1.465L19.071 12a5.008 5.008 0 0 0 0-7.071 5.006 5.006 0 0 0-7.071 0z"
            ></path>
          </svg>
        </div>
      </div>

      <!-- BOX LINK -->
      <div class="border-2 border-gray-200 flex justify-between items-center mt-4 py-2">
        <svg
          xmlns="http://www.w3.org/2000/svg"
          width="24"
          height="24"
          viewBox="0 0 24 24"
          class="fill-gray-500 ml-2"
        >
          <path
            d="M8.465 11.293c1.133-1.133 3.109-1.133 4.242 0l.707.707 1.414-1.414-.707-.707c-.943-.944-2.199-1.465-3.535-1.465s-2.592.521-3.535 1.465L4.929 12a5.008 5.008 0 0 0 0 7.071 4.983 4.983 0 0 0 3.535 1.462A4.982 4.982 0 0 0 12 19.071l.707-.707-1.414-1.414-.707.707a3.007 3.007 0 0 1-4.243 0 3.005 3.005 0 0 1 0-4.243l2.122-2.121z"
          ></path>
          <path
            d="m12 4.929-.707.707 1.414 1.414.707-.707a3.007 3.007 0 0 1 4.243 0 3.005 3.005 0 0 1 0-4.243l-2.122 2.121c-1.133 1.133-3.109 1.133-4.242 0L10.586 12l-1.414 1.414.707.707c.943.944 2.199 1.465 3.535 1.465s2.592-.521 3.535-1.465L19.071 12a5.008 5.008 0 0 0 0-7.071 5.006 5.006 0 0 0-7.071 0z"
          ></path>
        </svg>

        <input
          class="w-full outline-none bg-transparent"
          type="text"
          placeholder="link"
          bind:value={currentPageLink}
        />

        <button
          class="bg-indigo-500 text-white rounded text-sm py-2 px-5 mr-2 hover:bg-indigo-600"
          on:click={copyLink}
        >
          Copy
        </button>
      </div>
    </div>
  </div>
</div>
