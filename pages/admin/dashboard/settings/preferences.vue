<template>
  <div class="flex-auto">
    <div class="flex flex-col bg-gray-50">
      <div>
          <h4 class="font-bold text-black text-2xl pt-5 pl-5 pb-5">Accomodation Preferences</h4>
          <p class="text-gray-700 pl-5 pb-5">Customize your profile, theme, language, preferences... </p>
      </div>
  </div>
  <hr>
  <div class="py-10 px-5">
    <p class="font-bold text-gray-900">Academic Year</p>
    <div class="flex justify-between">
      <label for="toogleButton" class="flex items-center cursor-pointer">
         <div class="pr-56">Activate or deactivate criteria to select accomodation by academic year.</div>
     <!-- toggle -->
     <div class="relative">
       <input id="toogleButton" type="checkbox" class="hidden" />
       <!-- path -->
       <div
         class="toggle-path bg-gray-200 w-9 h-5 rounded-full shadow-inner"
       ></div>
       <!-- circle -->
       <div
         class="toggle-circle absolute w-3.5 h-3.5 bg-white rounded-full shadow inset-y-0 left-0"
       ></div>
     </div>
   </label>
  
  </div>
   
  </div>
    <hr>
    <div class="py-10 px-5">
      <p class="font-bold text-gray-900">Program</p>
      <div class="flex justify-between">
        <label for="toogleButtonByProgram" class="flex items-center cursor-pointer">
           <div class="pr-60">Activate or deactivate criteria to select accomodation by program.</div>
       <!-- toggle -->
       <div class="relative">
         <input id="toogleButtonByProgram" type="checkbox" class="hidden" />
         <!-- path -->
         <div
           class="toggle-path bg-gray-200 w-9 h-5 rounded-full shadow-inner"
         ></div>
         <!-- circle -->
         <div
           class="toggle-circle absolute w-3.5 h-3.5 bg-white rounded-full shadow inset-y-0 left-0"
         ></div>
       </div>
     </label>
    
    </div>
     
    </div>
    <hr>
    <div class="py-10 px-5">
      <p class="font-bold text-gray-900">Health problem</p>
      <div class="flex justify-between">
        <label for="toogleButtonByHealth" class="flex items-center cursor-pointer">
           <div class="mr-56">Activate or deactivate criteria to select accomodation by health issue.</div>
       <!-- toggle -->
       <div class="ml-50 relative">
         <input id="toogleButtonByHealth" type="checkbox" class="hidden" />
         <!-- path -->
         <div
           class="toggle-path bg-gray-200 w-9 h-5 rounded-full shadow-inner"
         ></div>
         <!-- circle -->
         <div
           class="toggle-circle absolute w-3.5 h-3.5 bg-white rounded-full shadow inset-y-0 left-0"
         ></div>
       </div>
     </label>
    
    </div>
     
    </div>
    <hr>
    
  </div>
</template>

<script>
export default {

}
</script>

<style>

.toggle-path {
    transition: background 0.3s ease-in-out;
}
.toggle-circle {
    top: 0.2rem;
    left: 0.25rem;
    transition: all 0.3s ease-in-out;
}
input:checked ~ .toggle-circle {
    transform: translateX(100%);
}
input:checked ~ .toggle-path {
    background-color:#81E6D9;
}
</style>