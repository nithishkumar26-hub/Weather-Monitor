<script>

  import { goto } from "$app/navigation";
  function goTo(){
    goto('/weather')
  }
  function goTonew(){
    goto('/weathernew')
  }

</script>
<!-- <h1>Welcome to SvelteKit</h1>
<p>Visit <a href="https://kit.svelte.dev">kit.svelte.dev</a> to read the documentation</p> -->

<div class="flex flex-col items-center p-[50px]">
    <button class="w-[280px] h-[130px] bg-pink-500 rounded-[30px] flex flex-row items-center 
    justify-center" on:click={()=>goTo()}>
        <div class="font-poppins font-medium text-[20px] ml-[20px] cursor-pointer"> Weather App1</div>
        <img src="/arrow-right-thick.svg" alt="right" class="h-[40px] w-[40px]"/>
    </button>
    <button class="w-[280px] h-[130px] mt-[50px] bg-pink-500 rounded-[30px] flex flex-row items-center 
    justify-center" on:click={()=>goTonew()}>
        <div class="font-poppins font-medium text-[20px] ml-[20px] cursor-pointer"> Weather App2</div>
        <img src="/arrow-right-thick.svg" alt="right" class="h-[40px] w-[40px]"/>
    </button>
</div>
