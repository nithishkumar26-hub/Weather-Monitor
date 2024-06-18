<script>
  import { theme } from "../../store";

  function changeTheme(){
    console.log("clicked",$theme)
    $theme=!$theme
  }
  
</script>



  <div class={`${$theme && "dark"}`} >
    <div class="font-poppins text-white  h-[300px] bg-[#44ad46] shadow-md rounded-xl text-center w-1/2 mx-auto  mt-[10%] dark:bg-slate-600">
      <p>create this page because for adding dark mode to my website</p>
      <button class="w-[200px] h-8 bg-yellow-600 rounded-md dark:bg-blue-400" on:click={()=> changeTheme()}>click me!</button>
    </div>
  </div>
 