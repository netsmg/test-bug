<script>
  import { onDestroy, onMount } from "svelte";
  import { collection, query, orderBy, startAfter, endAt, limit, onSnapshot, updateDoc, doc, increment, getDocs } from "firebase/firestore";
  import { fstore } from "../firebase";
  import { common } from "../stores/postStore";

  import PostList from "../lib/Components/PostList.svelte";
  import PostSkeleton from "../lib/Components/PostSkeleton.svelte";
  import Search from "../lib/Components/Search.svelte";
  import SectionHead from "../lib/Components/SectionHead.svelte";

  let posts = [];
  let lastVisible;
  let lim = 20;
  let postCount = 0;

  const makeData = (docS) => {
    posts = [];
    docS.forEach(doc => {
      posts.push({
        ...doc.data(),
        mins: common.getReadTime(doc.data().content),
        createdAt: common.getDate(doc.data().createdAt),
        id: doc.id
      });
    });
  };

  const fetchData = async (queryFn) => {
    posts = [];
    const q = queryFn();
    const docS = await getDocs(q);
    lastVisible = docS.docs[docS.docs.length - 1];
    makeData(docS);
    setTimeout(() => {
      MathJax.typeset();
    }, 500);
  };

  const next = () => {
    if (posts.length >= 10) {
      fetchData(() => query(
        collection(fstore, "posts"),
        orderBy("createdAt", "desc"),
        startAfter(lastVisible),
        limit(lim)
      ));
      postCount++;
    }
  };

  const prev = () => {
    if (postCount > 0) {
      fetchData(() => query(
        collection(fstore, "posts"),
        orderBy("createdAt", "desc"),
        endAt(lastVisible),
        limit(lim)
      ));
      postCount--;
    }
  };

  const readHandle = async (id, read) => {
    const postRef = doc(fstore, "posts", id);
    try {
      await updateDoc(postRef, {
        read: increment(1)
      });
    } catch (err) {
      console.log(err);
    }
  };

  onMount(async () => {
    document.title = "TESThub | Home";
    document.description = "An authentic blog to share your code";
    fetchData(() => query(
      collection(fstore, "posts"),
      orderBy("createdAt", "desc"),
      limit(lim)
    ));
  });

  onDestroy(() => {
    // Cleanup code here if needed
  });
</script>

<Search/>

<div class="hero min-h-82 max-w-screen-xl mx-auto py-3">
  <div class="hero-content flex-col lg:flex-row-reverse">
    <div class="text-center md:text-left md:mr-80">
      <h1 class="text-4xl font-bold">Hello 👋, I'm Al-amin</h1>
      <p class="py-3 text-xl">I enjoy building ideas into reality.</p>
    </div>

    {#if posts.length > 0}
      <SectionHead title="Most Recent"/>
      {#each posts as post, index}
        <a href="/blog/{post.id}" on:click={() => readHandle(post.id, post.read)}>
          <PostList
            read={post.read}
            commentCount={post.comment}
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
        {#if postCount > 0}
          <button on:click={prev} class="btn variant-filled-primary">Previous</button>
        {/if}
        {#if posts.length >= 10}
          <button on:click={next} class="btn variant-filled-primary">Next</button>
        {/if}
      </div>
    {:else}
      <PostSkeleton />
      <PostSkeleton />
      <PostSkeleton />
      <PostSkeleton />
    {/if}
  </div>
</div>
