<script>
  import { onMount } from "svelte";
  import { getDatabase, ref, push } from "firebase/database";

  let formData = {
    name: "",
    email: "",
    message: "",
  };

  const handleSubmit = async () => {
    const db = getDatabase();
    const contactsRef = ref(db, "contacts");

    // Push the form data to the "contacts" collection
    await push(contactsRef, formData);

    // Clear the form after submission
    formData = {
      name: "",
      email: "",
      message: "",
    };

    // Alert after submission
    alert("Form submit successful!");

    console.log("Form submitted to Firebase Realtime Database!");
  };
onMount(()=>{
        document.title = "Contact";
    })
</script>

<style>
  /* Add your Tailwind styles here */
</style>
<div class="text-2xl font-hind font-bold">Contact Form</div>
<div class="variant-glass p-5 rounded-md flex flex-col gap-2">
    <form on:submit={handleSubmit}>
  <label for="name">Name:</label>
  <input type="text" class="input input-bordered input-primary w-full max-w-xs border rounded-md w-full " id="name" bind:value={formData.name} />

  <label for="email">Email:</label>
  <input type="email" id="email" class="input input-bordered input-primary w-full  max-w-xsborder rounded-md w-full" bind:value={formData.email} />

  <label for="message">Message:</label>
  <textarea class="textarea textarea-primary" id="message" rows="4" bind:value={formData.message}></textarea>
  
  
  <button class="btn" type="submit">
  Send
  <svg xmlns="http://www.w3.org/2000/svg" class="h-6 w-6" fill="none" viewBox="0 0 24 24" stroke="currentColor"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M4.318 6.318a4.5 4.5 0 000 6.364L12 20.364l7.682-7.682a4.5 4.5 0 00-6.364-6.364L12 7.636l-1.318-1.318a4.5 4.5 0 00-6.364 0z" /></svg>
</button>

  
</form>

  
</div>

