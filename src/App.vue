<template>
<IncomeHeader :totalIncome="state.totalIncome"></IncomeHeader>
<IncomeForm  @add-income="AddIncome"></IncomeForm>
<IncomeList :state="state" @remove-item="removeItem"></IncomeList>
</template>


<script>
import {reactive,ref,computed} from "vue";
import IncomeHeader from "./components/IncomeHeader.vue";
import IncomeForm from "./components/IncomeForm.vue";
import IncomeItem from "./components/IncomeItem.vue";
import IncomeList from "./components/IncomeList.vue";
export default {
    components:{
      IncomeHeader,IncomeForm,IncomeItem,IncomeList
    },

    setup() {
      const state = reactive({
        income: [],
        totalIncome: computed(() => {
          let temp = 0;

          if(state.income.length > 0){
            for(let i = 0; i<state.income.length; i++){
              temp += state.income[i].value
            }
          }
          return temp;

        }),

        sortedIncome: computed(() => {
          let temp = []
          temp = state.income.sort(function (a,b){
            return b.date - a.date
          })
          return temp
        })

      })

       function AddIncome(data) {
          let d = data.date.split("-")
          let newD = new Date(d[0],d[1],d[2])

         state.income = [...state.income, {
           id: Date.now(),
           desc: data.desc,
           value: parseInt(data.value),
           date: newD.getTime()
         }]
         console.log(state.income)
       }


       function removeItem(id) {
        state.income = state.income.filter(v=> v.id != id)
       }

      return {
        state,AddIncome,removeItem
      }
    }
}

</script>


<style>
*{
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  font-family: 'Albert Sans', sans-serif;
}

body {
  background: #EEE;
}
</style>