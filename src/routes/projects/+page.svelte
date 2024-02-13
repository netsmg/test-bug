<script>
  import { collection, onSnapshot } from 'firebase/firestore';
  import { getFirestore } from 'firebase/firestore';

  let projects = [];
  const db = getFirestore();
  const query = collection(db, 'projects');

  onSnapshot(query, (snapshot) => {
    projects = snapshot.docs.map(doc => doc.data());
  });
</script>

<div class="max-w-screen-xl mx-auto mt-6 mb-4 px-4">
    <h2 class="text-2xl font-bold flex text-center md:text-left">Projects</h2>
    <div class="grid sm:grid-cols-2 md:grid-cols-3 lg:grid-cols-4 gap-3 mt-3 card w-96 bg-base-100 shadow-xl">
        <!-- Each project card -->
        {#if projects.length > 0}
            {#each projects as project (project.id)}
                <div class="min-w-full card-body">
                    <div class="bg-base-300 imgRoud shadow">
                        <a class="flex items-center gap-1 text-2xl mt-1">
                          <figure>  <img src="{project.image}" alt="{project.name}" class="min-w-full" data-atropos-offset="5"> </figure>
                        </a>
                    </div>
                    <h3>
                        <a href="{project.demo}" target="_blank" rel="noreferrer" class="flex items-center gap-1 text-2xl mt-1 card-title">{project.name} 
                            <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" strokeWidth="1.5" stroke="currentColor" class="hover:opacity-50 w-5 h-5">
                                <path strokeLinecap="round" strokeLinejoin="round" d="m4.5 19.5 15-15m0 0H8.25m11.25 0v11.25" />
                            </svg>
                        </a>
                    </h3>
                    <p>{project.description}</p>
                    {#each project.tags as tag}
                        <div class="badge badge-primary badge-outline">{tag}  </div>
                    {/each}
                </div>
            {/each}
        {/if}
        <!-- Repeat the above structure for each project -->
    </div>
</div>
