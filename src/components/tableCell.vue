<script setup>
 import {  defineProps,toRefs,computed} from 'vue';
 const props = defineProps({
  cellindex: Number,
  rowIndex:Number,
  cell:String,
  row:Object,
});
const { row , rowIndex } = toRefs(props);
 
const getDate = computed (()=>{
    let data = JSON.parse(JSON.stringify(row.value)); 
    const date = new Date(data.dob.date);
    let currentDate = date.toLocaleDateString();
    return currentDate;
})
const getSerial = computed(()=>{
    var getSerialNo =  JSON.stringify(rowIndex.value+1);
    if(getSerialNo <= 9){
        getSerialNo = "0" + getSerialNo;
    }
    return getSerialNo;
}); 
</script>
<template>  
  <td>
        <span v-if="cell == 'S/no'"> {{getSerial}}</span>
        <span v-if="cell=='name'">
            <slot :item="row.name"> </slot>           
        </span>
       <span v-if="cell=='dob'">
           {{getDate}}
        </span>
        <span v-if="cell=='email'"> {{row.email}}</span>
        <span v-if="cell=='location'">
           {{row.location.street.number}}  {{row.location.street.name}}  {{row.location.city}} 
           {{row.location.state}} {{row.location.country}} {{row.location.postcode}} 
        </span>
        <span v-if="cell=='phone'"> {{row.phone}}</span>
        <span v-if="cell=='picture'"> <img :src="row.picture.medium" alt="" />  </span>    
  </td>
</template>



<style>

</style>