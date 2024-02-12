<script>
  import Icon from '@iconify/svelte';

  export let avatar;
  export let author;
  export let title;
  export let express;
  export let createdAt;
  export let tags;
  export let mins;
  export let read;
  export let commentCount = 0;

  
</script>

<svelte:head>
  <link rel="stylesheet" href="./css/bootstrap-reboot.min.css">
  <link rel="stylesheet" href="./css/bootstrap-grid.min.css">
  <link rel="stylesheet" href="./css/owl.carousel.min.css">
  <link rel="stylesheet" href="./css/magnific-popup.css">
  <link rel="stylesheet" href="./css/nouislider.min.css">
  <link rel="stylesheet" href="./css/jquery.mCustomScrollbar.min.css">
  <link rel="stylesheet" href="./css/paymentfont.min.css">
  <link rel="stylesheet" href="./css/main.css"> <!-- Assuming main.css is your custom stylesheet -->
</svelte:head>

<section class="section main container">
  <div class="col-12 col-md-12 col-lg-6">
    <div class="post post--big">
      <a class="post__img">
        <div>{@html express}</div>
      </a>
      <div class="post__content">
        <div class="flex flex-wrap gap-2 mt-2 items-center text-gray-500 font-poppins text-sm max-sm:text-[10px]">
          {mins} min read ·
          {#each tags as tag, index}
            {#if index < 3}
              <code>{tag}</code>
            {/if}
          {/each}
        </div>
        <h3 class="post__title"><a>{title}</a></h3>
        <div class="post__meta">
          <span class="post__date">
            <svg xmlns='http://www.w3.org/2000/svg' width='512' height='512' viewBox='0 0 512 512'>
              <path d='M256,64C150,64,64,150,64,256s86,192,192,192,192-86,192-192S362,64,256,64Z' style='fill:none;stroke-miterlimit:10;stroke-width:32px'/>
              <polyline points='256 128 256 272 352 272' style='fill:none;stroke-linecap:round;stroke-linejoin:round;stroke-width:32px'/>
            </svg> {createdAt}
          </span>
          <span class="font-lato flex read gap-1 max-sm:text-[11px]">
            <Icon icon="ic:baseline-remove-red-eye" />{read} <Icon icon="bi:dot" />
          </span>
          <span class="post__comments">
            <svg xmlns='http://www.w3.org/2000/svg' width='512' height='512' viewBox='0 0 512 512'>
              <path d='M431,320.6c-1-3.6,1.2-8.6,3.3-12.2a33.68,33.68,0,0,1,2.1-3.1A162,162,0,0,0,464,215c.3-92.2-77.5-167-173.7-167C206.4,48,136.4,105.1,120,180.9a160.7,160.7,0,0,0-3.7,34.2c0,92.3,74.8,169.1,171,169.1,15.3,0,35.9-4.6,47.2-7.7s22.5-7.2,25.4-8.3a26.44,26.44,0,0,1,9.3-1.7,26,26,0,0,1,10.1,2L436,388.6a13.52,13.52,0,0,0,3.9,1,8,8,0,0,0,8-8,12.85,12.85,0,0,0-.5-2.7Z' style='fill:none;stroke-linecap:round;stroke-miterlimit:10;stroke-width:32px'/>
              <path d='M66.46,232a146.23,146.23,0,0,0,6.39,152.67c2.31,3.49,3.61,6.19,3.21,8s-11.93,61.87-11.93,61.87a8,8,0,0,0,2.71,7.68A8.17,8.17,0,0,0,72,464a7.26,7.26,0,0,0,2.91-.6l56.21-22a15.7,15.7,0,0,1,12,.2c18.94,7.38,39.88,12,60.83,12A159.21,159.21,0,0,0,284,432.11' style='fill:none;stroke-linecap:round;stroke-miterlimit:10;stroke-width:32px'/>
            </svg> {commentCount}
          </span>
        </div>
      </div>
    </div>
  </div>
</section>
