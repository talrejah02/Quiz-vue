<template>
 <div>

    <b-nav tabs >
       <b-nav-item disabled> <b> hitesh quiz app </b> </b-nav-item>
       <b-nav-item disabled> counter {{ numcorrect }} / {{ totalnum }} </b-nav-item>

  </b-nav>
 </div>
</template>

<script>
export default {
   props:[

      'numcorrect',
      'totalnum'
   ]
   

}
</script>