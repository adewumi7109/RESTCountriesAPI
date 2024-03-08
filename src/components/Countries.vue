<script setup>

</script>
<template>
    <div class="wrapper">
        <div class="filter">
            <div>
              <form @submit.prevent="search">
                <input type="text" v-model="searchInput" placeholder="search for a country..." />
              </form>
            </div>
            <div>
                <select v-model="selectFilter" @change="search">
                    <option value="all">Filter by Region</option>
                    <option value="Africa">Africa</option>
                    <!-- <option value="America">America</option> -->
                    <option value="Asia">Asia</option>
                    <option value="Europe">Europe</option>
                    <option value="Oceania">Oceania</option>
                </select>
            </div>
        </div>
        <div >
            <h1 class="notfound" v-if="countries.length ==0">
                No result found for the search criteria
            </h1>
            <div v-else>
                <div class="countries">
                    <div v-for="country in countries" :key="country.name" class="box">
            <img :src="country.flag" alt="" />
            <div class="details">
                <h1>{{ country.name }}</h1>
                <p><b>Population:</b> {{ country.population }}</p>
                <p><b>Region:</b> {{ country.region }}</p>
                <span><b>Capital:</b> {{ country.capital }}</span>
            </div>
        </div>
                </div>
        
    </div>
        </div>

    </div>
</template>

<script>
export default {
    data() {
        return {
            countries: [],
            searchInput: "",
            selectFilter: "all",
        }
    },
    methods: {
        search(){
            fetch('/src/data/data.json')
            .then(res => res.json())
            .then(data => {
                let searchTerm = this.searchInput.toLowerCase();
                let selectedRegion = this.selectFilter.toLowerCase();
                var result = data.filter(item => item.name.toLowerCase().includes(searchTerm));
                if(searchTerm.length == 0){
                    this.countries = data
                }
                if(selectedRegion != "all"){
                  result = data.filter(item => item.region.toLowerCase() === selectedRegion && item.name.toLowerCase().includes(searchTerm));

                  this.countries = result
                }
                else{
                    
                    this.countries = result
                }
            })
            .catch(err => console.log(err.message))
            console.log(this.selectFilter)
        }
    },
    mounted() {
        fetch('/src/data/data.json')
            .then(res => res.json())
            .then(data => {
                this.countries = data
                console.log(data)
            })
            .catch(err => console.log(err.message))
    },
    watch:{
        searchInput(newVal, oldVal ){
            if(newVal || oldVal){
                this.search()
            }
            
        
    
        }
    }
}
</script>
<style scoped>
.countries {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
    gap: 30px;
    margin-top: 40px;
    font-size: .9rem;

}
.box{
    box-shadow: rgba(99, 99, 99, 0.2) 0px 2px 8px 0px;
    background-color: white;
}

.countries img {
    width: 100%;
    min-height: 150px;
    max-height: 150px;
    object-fit: cover;
    background-size: cover;
}

.wrapper {
    margin: 50px auto 0 auto;
    width: 90%;

}
.countries h1{
    font-size: 1.4rem;
    font-weight: bolder;
    margin-bottom: 15px;
}
.countries b{
    font-weight: bold;
}
.countries p{
    margin-bottom: 8px;
}

.filter {
    display: flex;
    justify-content: space-between;
}

.filter input {
    height: 7vh;
    padding-left: 5px ;
    width: 250px;
}
.filter select{
    height: 7vh;
    padding: 0 7px ;

}
.details{
    padding: 20px;

}
.notfound{
    height: 50vh;
    display: flex;
    justify-content: center;
    align-items: center;
    font-weight: bolder;
}
</style>
