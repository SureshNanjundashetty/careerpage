<template>
  <section class="event-list-component py-12">
    <div v-if="groupedList.length>0">
    <div  v-for="dep in groupedList" :key="dep?.department" class="mt-5 mb-20 container mx-auto sm:w px-2">
        <div class="sm:w">
          <h3 class="event-inheading" v-if="dep?.department">{{dep?.department}}</h3>

          <ul>
          
            <li v-for="(item, index) in dep?.careers" :key="index"   >
               <a :href="item.url_active_page" target="_blank">
               <div v-show="item" class="flex events-list flex-row justify-between border-li" @mouseover="hovered(index)" @mouseout="unhovered">
              <div class="py-5 pr-3 careerName w-1/4">
                <span class="career-nm inline-block px-1 ">
                  {{ item.name }}
                </span>
              </div>
              <div  class="py-5 pr-3 w-1/4 ">
              <span class="event-date">
                <span v-show="item.location.state" >
                  {{item.location.state }}
                </span>
                <span v-show="item.location.country" >
                  {{item.location.country}}
                </span>
                </span>
              </div>
             
              <div class="py-5 px-4 country w-1/4"> {{ item.employment_type}} </div>
              <div class="py-5">
                <a v-if="item.url_active_page " :href="item.url_active_page" target="_blank" class="h-9 w-9  rounded-2xl flex justify-center items-center ">
                 OPEN <!-- <IconButton as="div" class="button" :icon="'ArrowRight'" :icon-bg-color="'Grey'" :icon-color="'Dark'" /> -->
                </a>
              </div>
              </div>
                  </a>
            </li>
        
      
          </ul>
         
        </div>
      <!-- </div> -->
    </div>
      </div>
  </section>

</template>
<script>
import Vue from 'vue';


export default Vue.extend({
  name: 'EventComponent',
  
  filters: {
     getDate(value) {
      const myArray = value.split('T');
      const myArray1 = myArray[0].split('-');
      const month = ['January', 'February', 'March', 'April', 'May', 'June', 'July', 'August', 'September', 'October', 'November', 'December'];
      const selectedMonth = month[Math.floor(myArray1[1] - 1)];
      const valueToReturn = selectedMonth + ' ' + myArray1[2] + ', ' + myArray1[0];
      return valueToReturn;
    },
    tagFormatting(value) {
      const tag = value.replaceAll('_', ' ');
      return tag;
    },
  },

  props: {
    careerslist: {
      type: Array,
      required: false,
      default: () => [],
    },
    department: {
      type: Array,
      required: false,
      default: () => [],
    },
  },

  data() {
    return {
isHovering: false,
currentHoverIndex:null,
nodataDepartment:[]
    };
  },

  computed: {
    groupedList(){
      const list=[]
      this.department.forEach(dep => {
        const newArray = this.careerslist.filter(function (el){ return el?.department === dep })
        if(newArray.length>0){
          list.push({department:dep, careers:newArray})
        }
      });
      return list;

    }
    

  },
 

  mounted() {
  },

  methods: {
//  grouped(dep){
//   if(this.careerslist?.length>0){
//         const newArray = this.careerslist.filter(function (el){ return el?.department === dep })
//           const index = this.nodataDepartment?.findIndex(v => v === dep) 
          
//         if(newArray?.length<1){
//       if ( index < 0)
//         this.nodataDepartment?.push(dep)   
//         }
//         else{
//           if(this.nodataDepartment)
//           this.nodataDepartment?.splice(index, 1)
//           }
//         return newArray;
//   }else{
//     return []
//   }
//     },
   
    hovered(val){
       this.isHovering= true;
      this.currentHoverIndex=val
    },
     unhovered(){
       this.isHovering= false;
       this.currentHoverIndex=null;
    }
  },
});
</script>
<style scoped>
ul{
  margin-top:12px;
}
.border-li{
  border-bottom:1px solid #8e90a6;
  padding:20px 0;
}
.country{
  font-weight: 400;
font-size: 16px;
line-height: 140%;
color: #555770;
@media (min-width:1024px){
  min-width:120px;
} 
}
.eventTitle {
  gap: 28px;
  font-weight: 700;
  font-size: 16px;
  line-height: 140%;
  color: #0E142C;
}
.hovering{
color: var(--color-brandBlue) !important;
}
.event-date{
  font-weight: 400;
font-size: 16px;
line-height: 140%;
color: #555770;
@media (min-width:1024px){
min-width: 200px;
display: inline-block;
}
}
.career-nm{

border-radius: 4px;
font-family: 'Moderat';
font-style: normal;
font-weight: 500;
font-size: 14px;
line-height: 100%;
letter-spacing: 0.002em;
color: #0E142C;
padding: 8px 12px;
gap: 10px;
}
.event-inheading{
  font-family: 'Moderat';
font-style: normal;
font-weight: 700;
font-size: 19px;
line-height: 140%;
color: #0E142C;
opacity:1 !important;
}
  .text-md {
    font-size: 14px;
  }
  @media (min-width:1200px){
.careerName {
    min-width: 145px;
}
  }
    @media (max-width:600px){
.career-nm,.country,.event-date{
  font-size:12px;
}
.eventTitle{
  font-size:13px;
}
    }
 
</style>
