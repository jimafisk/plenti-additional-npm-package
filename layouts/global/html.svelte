<script>
  import Head from './head.svelte';
  import Nav from './nav.svelte';
  import Footer from './footer.svelte';
  import { makeTitle } from '../scripts/make_title.svelte';

  export let content, layout, allContent, allLayouts, env, user, adminMenu;
</script>

<html lang="en">
<Head title={makeTitle(content.filename)} {env} />
<body>
  {#if user && $user.isAuthenticated}
      <svelte:component this={adminMenu} {user} bind:content={content} />
  {/if}
  <main>
    <Nav />
    <div class="container">
      <svelte:component this={layout} {...content.fields} {content} {allContent} {allLayouts} {user} />
      <br />
    </div>
    <Footer {allContent} />
  </main>
</body>
</html>

<style>
  html, body {
    height: 100%;
  }

  body {
    font-family: 'Rubik', sans-serif;
    display: flex;
    flex-direction: column;
    margin: 0;
  }
  main {
    flex: 1 0 auto;
  }
  :global(.container) {
    max-width: 1024px;
    margin: 0 auto;
    flex-grow: 1;
    padding: 0 20px;
  }
  :global(:root) {
    --primary: rgb(34, 166, 237);
    --primary-dark: rgb(16, 92, 133);
    --accent: rgb(254, 211, 48);
    --base: rgb(245, 245, 245);
    --base-dark: rgb(17, 17, 17);
  }
  .container :global(a) {
    position: relative;
    text-decoration: none;
    color: var(--base-dark);
    padding-bottom: 5px;
  }
  .container :global(p a:before) {
    content: "";
    width: 100%;
    height: 100%;
    background-image: linear-gradient(to top, var(--accent) 25%, rgba(0, 0, 0, 0) 40%);  
    position: absolute;
    left: 0;
    bottom: 2px;
    z-index: -1;   
    will-change: width;
    transform: rotate(-2deg);
    transform-origin: left bottom;
    transition: width .1s ease-out;
  }
  .container :global(a:hover:before) {
    width: 0;
    transition-duration: .15s;
  }
</style>
