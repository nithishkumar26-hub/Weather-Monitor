<script>
    // @ts-nocheck
    
        export let value = '';
        export let placeholder = '';
        export let type = 'text';
        export let id="";
        export let style="";
        // export let class='';
        function handleChange(event) {
          value = event.target.value;
        
        }
      </script>
      
      <label class="w-full">
        <input {type} {placeholder} {id}  {value}  class="{style} text-[#333333] font-medium font-poppins pl-3 
        text-[13px]  h-10 rounded-[10px] border border-[#CCCCCC]  mt-1 outline-none bg-[#ebfffc]
        pr-[10px] appearance-none "   on:input={handleChange} />
      </label>

      <!-- focus:outline-[#6E36FF]  -->