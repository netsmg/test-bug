<script>
  import { onMount } from "svelte";
  import { collection, getDocs, query, orderBy } from "firebase/firestore";
  import { getFirestore } from 'firebase/firestore';

  let products = [];
  const db = getFirestore();

  let searchTerm = '';
  let searchResults = [];

  const searchPosts = async () => {
    searchResults = [];

    const q = query(
      collection(db, "posts"),
      orderBy("createdAt", "desc")
    );

    const querySnapshot = await getDocs(q);

    querySnapshot.forEach((doc) => {
      const post = {
        ...doc.data(),
        id: doc.id
      };

      const lowerCaseSearchTerm = searchTerm.toLowerCase();
      const postMatchesSearch =
        post.title.toLowerCase().includes(lowerCaseSearchTerm) ||
        post.content.toLowerCase().includes(lowerCaseSearchTerm) ||
        post.tags.some((tag) => tag.toLowerCase().includes(lowerCaseSearchTerm));

      if (searchTerm === '' || postMatchesSearch) {
        searchResults.push(post);
      }
    });

    // Open the modal when search results are available
    if (searchResults.length > 0) {
      openModal();
    }
  };

  let modalOpen = false;

  const openModal = () => {
    modalOpen = true;
  };

  const closeModal = () => {
    modalOpen = false;
  };

  
</script>


<div>
    
<div class="variant-glass p-5 rounded  z-0 w-full">
    <div class="animate__animated animate__fadeIn text-3xl max-sm:text-xl font-fold font-bold mb-2 drop-shadow-lg">
        <span class="bg-gradient-to-br from-blue-500 to-cyan-400 bg-clip-text text-transparent box-decoration-clone">Search</span>
        <span class="bg-gradient-to-br from-red-500 to-yellow-500 bg-clip-text text-transparent box-decoration-clone">content</span>
        <span class="bg-gradient-to-br from-pink-500 to-violet-500 bg-clip-text text-transparent box-decoration-clone">here</span>
       </div>

       <div class="flex font-poppins items-center justify-between ">
    
  <input bind:value={searchTerm} class="input border rounded-md w-full" 
 placeholder="Search posts" on:input={searchPosts} />
  <!-- No need for a separate search button -->

  {#if searchResults.length > 0}
    <button on:click={openModal} class="mt-5 bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded">Open Results</button>
  {/if}

  {#if modalOpen}
    <div class="modal">
      <div class="modal-content">
        <span on:click={closeModal} class="close">&times;</span>
        
        <h2>Search Results</h2>
        
        {#each searchResults as result, index}
          <div>
            <p><a href="/blog/{result.id}">{result.title}</a></p>
            <!-- Display other post details as needed -->
          </div>
        {/each}
      </div>
    </div>
  {/if}



    </div>
</div>

</div>


  
  
 

<style>
  /* Modal styles */
  .modal {
    display: block;
    position: fixed;
    z-index: 1;
    left: 0;
    top: 0;
    width: 100%;
    height: 100%;
    overflow: auto;
    background-color: rgba(0, 0, 0, 0.4);
    padding-top: 60px;
  }
  

  .modal-content {
    background-color: #fefefe;
    margin: 5% auto;
    padding: 20px;
    border: 1px solid #888;
    width: 80%;
  }

  .close {
  
    color: #aaa;
    float: right;
    font-size: 28px;
    font-weight: bold;
  }

  .close:hover,
  .close:focus {
    color: black;
    text-decoration: none;
    cursor: pointer;
  }
</style>
