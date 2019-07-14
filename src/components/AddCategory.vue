<template>
    <form @submit="addCategories" class="add-category-form">
        <input type="submit" value="+" class="btn">
        <label for="categories" class="category">Category:</label>
        <select v-model="selected" :disabled="isCustom"
        :class="!isComplete ? 'categories uncomplete' : 'categories'" id="categories">
            <option value="--Choose Category--" selected disabled>--Choose Category--</option>
            <option value="Sleep">Sleep</option>
            <option value="Eat">Eat</option>
            <option value="Family">Family</option>
            <option value="Friends">Friends</option>
            <option value="Romantic Relationship">Romantic Relationship</option>
            <option value="Children">Children</option>
            <option value="Spouse">Spouse</option>
            <option value="Pets">Pets</option>
            <option value="Work">Work</option>
            <option value="Volunteering">Volunteering</option>
            <option value="Exercise / Movement">Exercise / Movement</option>
            <option value="Physical Health">Physical Health</option>
            <option value="Mental Health">Mental Health</option>
            <option value="Community Involvement">Community Involvement</option>
            <option value="Chores">Chores</option>
            <option value="Hobbies">Hobbies</option>
            <option value="Errands">Errands</option>
            <option value="Fun">Fun</option>
            <option value="Recreation">Recreation</option>
            <option value="Spiritual Health">Spiritual Health</option>
            <option value="School / Learning">School / Learning</option>
            <option value="Self-Care">Self-Care</option>
            <option value="Creative Outlets">Creative Outlets</option>
            <option value="Down Time">Down Time</option>
            <option value="Relaxation">Relaxation</option>
        </select>
        <label for="custom">Custom:</label>
        <input type="text" class="custom" placeholder="Custom Category" v-model="custom" id="custom">
        <label for="color">Color:</label>
        <input type="color" class="color" v-model="color" id="color">
        <label for="percentage">% of Time:</label>
        <input type="number" class="percentage" v-model="percentage" id="percentage">
    </form>
</template>

<script>
import uuid from 'uuid';

export default {
    name: "AddCategories",
    props:["remainingPercentage", "categoriesInUse"],
    data(){
        return {
            selected: '--Choose Category--',
            custom: '',
            color: '#'+(0x1000000+(Math.random())*0xffffff).toString(16).substr(1,6),
            percentage: 0
        }
    },
    computed: {
        isComplete () {
            if (this.selected === "--Choose Category--" && this.custom === ""){
                return "Please choose a category or enter your own in the Custom field"
            }else if (this.percentage <= 0 || this.percentage > this.remainingPercentage){
                return "Please enter a number between 0% and " + this.remainingPercentage + "%";
            }else if (this.color === "#ffffff"){
                return "Please pick a color other than white";
            }else if (this.categoriesInUse.indexOf(this.selected) !== -1 || this.categoriesInUse.indexOf(this.custom) !== -1 ){
                return "Please use a category that isn't in use";
            }else if (this.custom.length > 50){
                return "Please use a category name that is less than 50 characters";
            }else{
                return true;
            }
        },
        isCustom() {
            return this.custom !== '';
        },
    },
    methods: {
        addCategories(e){
            if(this.isComplete === true){
                e.preventDefault();
                var cat = this.isCustom ? this.custom : this.selected
                const newCategories = {
                    id: uuid.v4(),
                    category: cat,
                    color: this.color,
                    percentage: parseInt(this.percentage)
                }
                //Send up to parent
                var selectobject = document.getElementById('categories');
                for (var i=0; i<selectobject.length; i++){
                    if (selectobject.options[i].value == cat ){
                        selectobject.remove(i);
                    }
                }
                this.$emit('add-categories', newCategories);
                this.selected = "--Choose Category--";
                this.custom = "";
                this.color = '#'+(0x1000000+(Math.random())*0xffffff).toString(16).substr(1,6);
                this.percentage = 0;
            }else{
                alert(this.isComplete)
            }
        }
    }
}
</script>

<style scoped>
    form {
        display: grid;
        grid-template-columns: .20fr .7fr 2fr;
        grid-template-rows: repeat(4, 1.75em);
        background-color: #f3f0f5;
        padding: 10px 0;
    }
    label{
        grid-column: 2;
        text-align: right;
        justify-content: center;
        align-self: center;
    }
    input, select {
        font-family: 'Avenir', Helvetica, Arial, sans-serif;
        grid-column: 3
    }

    .btn {
        font-size: xx-large;
        font-weight: 100;
        width: 100%;
        height: 116.5%;
        margin: 0;
        padding:0;
        color: black;
        background-color: #e9dff7;
        grid-column: 1;
        grid-row: 1/5;
        -webkit-appearance: none;
    }

    .btn:disabled {
        border-color:#d8d6d6;
        color: #d8d6d6;
    }

    .categories, .custom, .percentage, .btn {
        justify-content: center;
        align-self: center;
        border: 1px solid #d8d6d6;
    }

    .categories, .custom, .percentage{
        flex: 1 1 auto;
        border-radius: 5px;
        background-color: white;
    }

    .categories, .color, .custom, .percentage{
        margin: 0 10px;
    }

    .color {
        width:1.75em;
    }

    .custom {
        text-align: center
    }
    .percentage {
        text-indent: 5px;
    }

    .del{
        background: transparent;
        color: #000;
        border: none;
        cursor: pointer;
        vertical-align: text-top;
    }

    @media only screen and (max-width: 600px) {
        form {
            grid-template-columns: .30fr 1fr 2fr;
        }
    }
</style>


