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

<div>
  {#if projects.length > 0}
    {#each projects as project (project.id)}
      <div class="card w-96 bg-base-100 shadow-xl">
        <figure><img src="{project.image}" alt="{project.name}" /></figure>
        <div class="card-body">
          <h2 class="card-title">
            {project.name}
            {#each project.tags as tag}
              <div class="badge badge-secondary">{tag}  </div>
            {/each}
          </h2>
          <p>{project.description}</p>
          <div class="card-actions justify-end">
            <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" strokeWidth="1.5" stroke="currentColor" class="hover:opacity-50 w-5 h-5">
              <path strokeLinecap="round" strokeLinejoin="round" d="m4.5 19.5 15-15m0 0H8.25m11.25 0v11.25" />
            </svg>
            <div class="badge badge-outline"><a href="{project.demo}">Demo</a></div> 
            <div class="badge badge-outline"><a href="{project.code}">Code</a></div>
          </div>
        </div>
      </div>
    {/each}
  {/if}
</div>
