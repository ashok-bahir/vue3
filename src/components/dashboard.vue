<script setup>
import axios from 'axios';
import tableRow from './tableRow'; 
import { onMounted, onUpdated , ref } from 'vue';
 
  const userData = ref([]);
  var headers= ["S/no","name","dob","email","location","phone","picture"];
  onMounted(()=>{    
    axios.get(`https://randomuser.me/api/?results=50`)
      .then(res=>{
        let temp = JSON.stringify(res);
        let temp_obj = JSON.parse(temp);
        userData.value = temp_obj.data.results;
        console.log(userData.value);
      });
  }),
  onUpdated(()=>{
    function sortTableByColumn(table, column, asc = true) {
        const dirModifier = asc ? 1 : -1;
        const tBody = table.tBodies[0];
        const rows = Array.from(tBody.querySelectorAll("tr"));
        const sortedRows = rows.sort((a, b) => {
            const aColText = a.querySelector(`td:nth-child(${ column + 1 })`).textContent.trim();
            const bColText = b.querySelector(`td:nth-child(${ column + 1 })`).textContent.trim();
            return aColText > bColText ? (1 * dirModifier) : (-1 * dirModifier);
        });
        while (tBody.firstChild) {
            tBody.removeChild(tBody.firstChild);
        }
        tBody.append(...sortedRows);
        table.querySelectorAll("th").forEach(th => th.classList.remove("th-sort-asc", "th-sort-desc"));
        table.querySelector(`th:nth-child(${ column + 1})`).classList.toggle("th-sort-asc", asc);
        table.querySelector(`th:nth-child(${ column + 1})`).classList.toggle("th-sort-desc", !asc);
    }

    document.querySelectorAll(".table-sortable th").forEach(headerCell => {
        headerCell.addEventListener("click", () => {
            const tableElement = headerCell.parentElement.parentElement.parentElement;
            const headerIndex = Array.prototype.indexOf.call(headerCell.parentElement.children, headerCell);
            const currentIsAscending = headerCell.classList.contains("th-sort-asc");
            sortTableByColumn(tableElement, headerIndex, !currentIsAscending);
        });   
    });
  
    var flag = false;
      if(flag == false){
        flag = true;
        let gettable = document.querySelector("table.table-sortable");
        sortTableByColumn(gettable, 0, false);
      }
  })
</script>
<template>
  <div>
      <table class="table-sortable">
        <thead> 
        <tr>
          <th  v-for="(name,ind) in headers" :key="ind">{{name}}</th>
        </tr>
        </thead>
        <tbody>
        <tableRow v-for="(row,rowIndex) in userData" :key="rowIndex" :row="row" :rowindex="rowIndex" :headers="headers">
     
        </tableRow>
        </tbody>
      </table>
  </div>
</template>
<style>

.table-sortable th {
  padding-top: 12px;
  padding-bottom: 12px;
  text-align: left;
  background-color: #14aebe;
  color: white;
}

.table-sortable th {
  cursor: pointer;
}
.table-sortable tr img{
  border-radius: 50%;
}
.table-sortable .th-sort-asc::after {
  content: "\25b4";
}

.table-sortable .th-sort-desc::after {
  content: "\25be";
}

.table-sortable .th-sort-asc::after,
.table-sortable .th-sort-desc::after {
  margin-left: 5px;
}

.table-sortable .th-sort-asc,
.table-sortable .th-sort-desc {
  background: #077c88;
}
.table-sortable td{   
  padding: 5px;
}
.table-sortable td:nth-child(2){ 
  font-weight: bold;
}
 .table-sortable th {
   padding: 10px 5px;
 }
  .table-sortable th:nth-child(5) {
    min-width: 300px;
  }

.table-sortable tr:nth-child(even){background-color: #f2f2f2;}

.table-sortable tr:hover {background-color: #ddd;}



</style>