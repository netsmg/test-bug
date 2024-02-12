

<script >
  import { onDestroy, onMount } from "svelte";
  import { themes } from '$lib/themes';
  import PostList from "../lib/Components/PostList.svelte";
  import { common } from "../stores/postStore";
  import PostSkeleton from "../lib/Components/PostSkeleton.svelte";
  import Search from "../lib/Components/Search.svelte";
  
  import SectionHead from "../lib/Components/SectionHead.svelte";
  import {
    collection,
    query,
    orderBy,
    startAt,
    endAt,
    startAfter,
    endBefore,
    limit,
    onSnapshot,
    updateDoc,
    doc,
    increment,

    getDocs

  } from "firebase/firestore";
  import { fstore } from "../firebase";

let currentTheme = '';

  let posts = [];
  let lastVisible;
  let lim = 20;
  let postCount = 0;

  const makeData = (docS) => {
    docS.forEach(doc=>{
      posts.push({
          ...doc.data(),
          mins: $common.getReadTime(doc.data().content),
          createdAt: $common.getDate(doc.data().createdAt),
          id: doc.id
        });
    })

      posts = posts;
}

  const  nextData = async(lastVisible, lim) => {
      posts = [];
    const q = query(
      collection(fstore, "posts"),
      orderBy("createdAt", "desc"),
      startAfter(lastVisible),
      limit(lim)
    );

    postCount++;

    const docS = await getDocs(q);
    lastVisible = docS.docs[docS.docs.length-1];
    makeData(docS);

      setTimeout(()=>{
        MathJax.typeset();
      }, 500);
  }

  const  prevData = async(lastVisible, lim) => {
      posts = [];
    const q = query(
      collection(fstore, "posts"),
      orderBy("createdAt", "desc"),
      endAt(lastVisible),
      limit(lim)
    );

    postCount--;

    const docS = await getDocs(q);
    lastVisible = docS.docs[docS.docs.length-1];
    makeData(docS);

      setTimeout(()=>{
        MathJax.typeset();
      }, 500);
  }



  onMount(async ()=>{
    document.title = "PWTBLOG | Home";
    document.description = "An authentic blog to share your code";
    
    if (typeof window !== 'undefined') {
			const theme = window.localStorage.getItem('theme');
			if (theme && themes.includes(theme)) {
				document.documentElement.setAttribute('data-theme', theme);
				currentTheme = theme;
			}
		}
	});

	function setTheme(event: Event) {
		const select = event.target as HTMLSelectElement;
		const theme = select.value;
		if (themes.includes(theme)) {
			const one_year = 60 * 60 * 24 * 365;
			window.localStorage.setItem('theme', theme);
			document.cookie = `theme=${theme}; max-age=${one_year}; path=/; SameSite=Strict;`;
			document.documentElement.setAttribute('data-theme', theme);
			currentTheme = theme;
		}
    posts = [];
    const q = query(
      collection(fstore, "posts"),
      orderBy("createdAt", "desc"),
      limit(lim)
    );

    const docS = await getDocs(q);
    lastVisible = docS.docs[docS.docs.length-1];
    makeData(docS);

      setTimeout(()=>{
        MathJax.typeset();
      }, 500);
  });

  const readHandle = async(id, read) => {
    const postRef = doc(fstore, "posts", id);
    try{
      const res = await updateDoc(postRef, {
        read: increment(1)
      })
      // console.log(res);
    } catch(err){
      console.log(err);
    }
  }

  const next = () => {
      if(posts.length>=10){
        nextData(lastVisible, lim);
      }
  }

  const prev = () => {
      prevData(lastVisible, lim);
  }
  
 
</script>


<Search/>

<div class="absolute z-[-1] lg:left-[0] lg:top-[10px] top-[50px] md:top-10 md:left-[250px]">
  
</div>


{#if posts.length > 0}
<SectionHead title="Most Recent"/>
  {#each posts as post, index}
    <a href="/blog/{post.id}">
      <PostList
      on:click={()=> readHandle(post.id, post.read)}
        read={post.read}
        commentCount = {post.comment}
        author={post.author}
        title={post.title}
        express={post.express}
        tags={post.tags}
        createdAt={post.createdAt}
        mins={post.mins}
        avatar="https://codebuckblog.vercel.app/_next/image?url=%2F_next%2Fstatic%2Fmedia%2FLogo.e0b9e0a0.png&w=128&q=75"
      />
    </a>
  {/each}
  <div class="flex items-center justify-center gap-5">
    {#if postCount!=0}
    <button on:click={()=> prev()} class="btn variant-filled-primary">Previous</button>
    {/if}

    {#if posts.length>9}
    <button on:click={()=> next()} class="btn variant-filled-primary">Next</button>
    {/if}
  </div>
{:else}
  <PostSkeleton />
  <PostSkeleton />
  <PostSkeleton />
  <PostSkeleton />
{/if}

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
