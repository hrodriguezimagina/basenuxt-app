<template>
</template>
<script setup>


function addScript(w,d,u){
  var s=d.createElement('script');s.async=true;s.src=u+'?'+(Date.now()/60000|0);
  var h=d.getElementsByTagName('script')[0];h.parentNode.insertBefore(s,h);
}

onMounted(() => {
    addScript(window, document, 'https://cdn.bitrix24.com/b16889623/crm/site_button/loader_2_3zet30.js');
})


</script>

