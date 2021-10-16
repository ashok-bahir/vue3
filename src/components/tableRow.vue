<script setup>
import tableCell from './tableCell';
 import {  defineProps } from 'vue';

 defineProps({
  row: Object,
  rowindex:Number,
  headers:Array
})
</script>
<template>
  <tr>
      <tableCell v-for="(cell,cellIndex) in  headers" :key="cellIndex" :rowIndex="rowindex" :row="row" :cellindex="cellIndex" :cell="cell">
          <template v-slot:default="slotProps">
             {{slotProps.item.title}} {{slotProps.item.first}} {{slotProps.item.last}}  
          </template>
      </tableCell>
  </tr>
</template>
<style>

</style>