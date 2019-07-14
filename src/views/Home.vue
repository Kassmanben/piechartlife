<template>
<div>
  <div class="home">
    <h1> How are you prioritizing your life? </h1>
    <p>
      Choose the categories you feel like represent your average day/week <br><br>
      You can create your own category if you don't see it on the list. <br><br>
    </p>
    <AddCategory v-bind:remainingPercentage="remainingPercentage" v-bind:categoriesInUse="categoriesInUse" v-on:add-categories="addCategory"/>
    <div class="remaining-percentage">Percentage Remaining: {{remainingPercentage}}%</div>
    <div class="categories">
      <form class="category" v-bind:key="category.id" v-for="(category, index) in categories">
        <span  @click="deleteCategory(category.id, category.category)" class="deleteCategory"><font-awesome-icon icon="times-circle" /></span>
        <span :style="{'background-color': category.color}" class="color">_</span>
        <input type="text" maxlength="30" size="15" v-model="category.category" class="name">
        <div class="percentage">
          <span  @click="updatePercentage(index,'-')" :class="category.percentage <= 0 ? 'update-button disabled' : 'update-button'"><font-awesome-icon icon="minus-circle" /></span>
          {{category.percentage}} %
          <span class="update-button" @click="updatePercentage(index,'+')" :class="remainingPercentage <= 0 ? 'update-button disabled' : 'update-button'"><font-awesome-icon icon="plus-circle" /></span>
        </div>
      </form>
    </div>
    <Chart v-bind:chartData="chartData" />
  </div>
</div>
</template>

<script>
import AddCategory from "../components/AddCategory";
import Chart from "../components/Chart";
import Vue from 'vue';
import { library } from '@fortawesome/fontawesome-svg-core'
import { faMinusCircle, faPlusCircle, faTimesCircle } from '@fortawesome/free-solid-svg-icons'
import { FontAwesomeIcon } from '@fortawesome/vue-fontawesome'

library.add(faMinusCircle)
library.add(faPlusCircle)
library.add(faTimesCircle)

Vue.component('font-awesome-icon', FontAwesomeIcon)

export default {
  name: 'home',
  components: {
    AddCategory,
    Chart
  },
  computed: {
    remainingPercentage(){
      return 100 - this.categories.reduce((a, b) => a + (parseInt(b["percentage"]) || 0), 0)
    },
    categoriesInUse(){
      return this.categories.map((c) => {
              return c.category
          })
    },
    chartData() {
      var labels = this.categories.map((c) => {
              return c.category
          })
      var colors = this.categories.map((c) => {
                return c.color
            })
      var data = this.categories.map((c) => {
                return c.percentage
            })
      labels.push("Other");
      colors.push("#efefef");
      data.push(this.remainingPercentage);
      return {
        labels: labels,
        datasets: [
          {
            label: 'Time Use',
            backgroundColor: colors,
            data: data
          }
        ]
    }
    }
  },
  data(){
    return {
      categories: [],
    }
  },
  methods: {
    addCategory(newCategory){
      this.categories = [...this.categories, newCategory];
    },
    deleteCategory(id, category){
      var selectobject = document.getElementById('categories');
      var option = document.createElement('option');
      option.value = category; 
      option.innerText = category;
      selectobject.appendChild(option);
      this.categories = this.categories.filter(category => category.id !== id);
    },
    updatePercentage(index, sign){
      if(sign === "+" && this.remainingPercentage > 0){
        this.categories[index].percentage = parseInt(this.categories[index].percentage) + 1;
      }
      if(sign === "-" && this.categories[index].percentage > 0){
        this.categories[index].percentage = parseInt(this.categories[index].percentage) - 1;
      }
    }
  }
}
</script>

<style scoped>
  p {
    font-size: large;
  }

  input {
    border: 0;
    font-family: inherit;
    font-size: inherit;
    background-color: transparent
  }

  .home {
    width: 40%;
    display:inline-block
  }

  .categories {
    padding: 0 0 1em;
  }

  .category {
    display: grid;
    grid-template-columns: .1fr .4fr 1fr 1fr;
  }

  .category:hover {
    background-color: #eeeeee
  }

  .color {
    color: transparent;
    background-color: rgb(253, 209, 221);
    padding: 0 20px;
    margin: 0 20px;
    grid-column: 2;
  }

  .remaining-percentage {
    margin: 10px 0;
  }

  .update-button {
    vertical-align: middle; 
    color: #757575;
    margin: 0 6px;
  }

  .disabled {
    color: #eeeeee;
  }

  .deleteCategory {
    grid-column: 1;
    margin: 3px 0 0 0;
  }

  .name {
    grid-column: 3;
    text-overflow:ellipsis;
  }

  .percentage {
    grid-column: 4;
    text-align: left;
    margin: 3px 0 0 0;
  }

  @media only screen and (max-width: 600px) {
  .home {
    width: 100%;
  }
}
</style>

