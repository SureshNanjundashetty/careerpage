<template>
 
    <div  style="position: relative; z-index: 1">
   
      <!-- <Section :theme="'Light'"> -->
      <div ref="careerTable"  style="background: #fff; border-radius: 36px">
        <div ref="theVideo" class="mt-5 sm:flex sm:flex-row gap-4 cards tags-post">
          <div class="sm:basis-1/4 filter-container" style="margin: 12px; font-size: 14px; padding-top: 20px">
            <div class="grid grid-cols-1" style="padding-bottom: 18px">
              <div style="cursor: pointer">
                <span class="categories"> Open Positions </span>
              </div>
            </div>

            <div v-show="categoryTagsToggle" class="mb-8">
              <div class="mobile-flex">
                <div>
                  <input type="checkbox" class="form-checkbox h-5 w-5 text-gray-600" style="cursor: pointer" :checked="categoryTags?.length == 0" @change="includeAllDeparment($event)" />
                  <span :class="categoryTags?.length === 0 ? 'selected-tag' : 'tags'"> Show all </span>
                </div>
                <div v-for="item in allDepartments" :key="item" style="line-height: 28px">
                  <input :checked="categoryTags?.includes(item)" type="checkbox" :value="item" style="cursor: pointer" @click="includeDepartmentData(item, $event)" />
                  <span :class="'tags'">
                    {{ item }}
                  </span>
                </div>
              </div>
            </div>

            <div class="grid grid-cols-2" style="padding-bottom: 18px">
              <div style="cursor: pointer">
                <span class="categories"> locations </span>
              </div>
            </div>

            <div v-show="categoryTagsToggle" class="mb-8">
              <div class="mobile-flex">
                <div>
                  <input type="checkbox" class="form-checkbox h-5 w-5 text-gray-600" style="cursor: pointer" :checked="locationTags.length == 0" @change="includeAllLocation($event)" />
                  <span :class="locationTags.length === 0 ? 'selected-tag' : 'tags'"> Show all </span>
                </div>
                <div v-for="item in allLocations" :key="item" style="line-height: 28px">
                  <input v-model="locationTags" type="checkbox" :value="item" style="cursor: pointer" @click="includeLocationData(item, $event)" />
                  <span :class="locationTags.includes(item) === true ? 'selected-tag' : 'tags'">
                    {{ item }}
                  </span>
                </div>
              </div>
            </div>
          </div>
          <div class="sm:basis-3/4">
            <div class="mt-5 grid sm:grid md:grid lg:grid cardsss" style="margin: 0%; width: 87%">
              <CareerCard :career-details="filteredPost" :department="departmentTags" />
              <div v-if="filteredPost?.length !== 0 && filteredPost?.length < allCareers?.length" class="mt-8 pt-4 flex justify-center" style="margin-bottom: 36px">
                <div class="flex loadbtn">
                  <div class="load-more-arrow-icon" @click="morePost">
                    <span class="load-more-text" @click="morePost"> Load More </span>
                    <span class="svg-down">
                      <svg data-v-6751a3dc="" xmlns="http://www.w3.org/2000/svg" width="22" height="22" fill="none" viewBox="0 0 13 13" class="icon" style="width: 14px; height: 14px">
                        <path fill="#333" d="m11.43 7.438.354.353.853-.854H11.43v.5Zm-4.492 5.199 4.846-4.846-.707-.707L6.23 11.93l.707.707Zm4.492-5.7H.89v1h10.54v-1Z"></path>
                        <path stroke="currentColor" d="m5.852 1.313 4.33 4.33"></path>
                      </svg>
                    </span>
                  </div>
                </div>
              </div>
            </div>
            <!-- <div v-if="filteredPost.length === 0" class="lg:col-start-2 col-span-2" style="margin: 4%">
              <h1 style="font-size: 30px; color: red">Loading...</h1>
            </div> -->
          </div>
        </div>
      </div>
  <div class="container xl:px-60 py-24 mx-auto text-white">
    <div class="flex flex-wrap -m-12">
      <div class="p-12 md:w-1/2 flex flex-col items-start">
        <span class="inline-block whyJoinText">Why Join?</span>
        <div class="midTitle2">Company Benefits</div>

      </div>
      <div  class="comanyBenefit px-12 md:w-1/2  py-20 flex flex-col items-start">
        <p class="leading-relaxed mb-8">We offer competitive and comprehensive benefits, including:</p>
       <ul class="list-disc mb-4">
            <li class="mb-8">Health, vision and dental coverage including flexible spending accounts</li>
            <li class="mb-8">Unlimited vacation </li>
            <li class="mb-8">Pre-tax commuter benefits</li>
        </ul>
      </div>
    </div>

    <div class="flex flex-wrap -m-12">
      <div class="p-12 md:w-1/2 flex flex-col items-start">
        <div class="midTitle2">We’re building a diverse team</div>

      </div>
      <div  class="comanyBenefit px-12 md:w-1/2 py-16 flex flex-col items-start">
        <p class="leading-relaxed mb-8">VAST is proud to be an equal opportunity employer.</p>
        <p class="leading-relaxed mb-8"> We do not discriminate based upon race, religion, height, color, national origin, private stuff like sex and gender, age, disability, former employment or the like.</p>
      
      </div>
    </div>
  </div>
    </div>

</template>

<script>
import Vue from 'vue';

 import CareerCard from '~/components/CareerCard.vue';

export default Vue.extend({
  name: 'DynamicPage',

 components:{
  CareerCard
 },

  data() {
    return {
      link: {},
      label: '',
      theme: 'Dark',
      title: 'Join us for events featuring diverse voices & innovative ideas',
      recentPost: [],
      selectedTagIndex: 0,
      allTags: [],
      allTagsValue: [],
      filteredPost: [],
      count: 0,
      categoryTagsToggle: true,
      categoryTags: [],
      locationTags: [],
      heroEventsData: [],
      dataLoaded: false,
      defaultGradientData: {
        fields: {
          title: 'Blue',
          colors: ['#1498F7', '#0F32BB', '#0d3a5e', ' #010e2a'],
        },
      },
      allDepartments: [],
      allLocations: [],
      allCareers: [],
      locationShowall: true,
      departmentShowall: true,
      info: [],
    };
  },

  computed: {
    departmentTags() {
      if (this.departmentShowall) {
        return this.allDepartments;
      } else {
        return this.categoryTags;
      }
    },
    darkTheme() {
      return 'Dark';
    },

    sortedAllTags() {
      return this.sortedTags();
    },

    gradientColors() {
      // const { gradientColor } = (this.fields || {}) as TypePageFields;
      return this.defaultGradientData;
    },

  },

  watch: {
    // info: {
    //   handler() {
    //    // this.loadData();
    //     this.refreshData();
    //   },
    // },
    locationTags: {
      immediate: true,
      handler() {
        this.refreshData();
      },
    },

    categoryTags: {
      immediate: true,
      handler() {
        this.refreshData();
      },
    },

    allCareers: {
      immediate: true,
      handler() {
        this.filteredPost = this.allCareers.slice(0, 10);
      },
    },
  },


  mounted() {
     this.loadApi();
  },

 

  methods: {
    scrollMeTo() {
      const element = this.$refs.careerTable;
      const top = element.offsetTop;

      window.scrollTo(0, top);
    },

    async loadApi() {
      const response = await fetch('https://www.comeet.co/careers-api/2.0/company/43.001/positions?token=34110453411D4968234168209C31D49').then((res) => res.json());
      
      this.info = response;
      alert(this.info)
      //this.loadData()
    },

    includeAllDeparment($event) {
      const checked = $event.target.checked;
      if (checked) {
        this.departmentShowall = true;
        this.categoryTags = [];
      } else {
        this.departmentShowall = false;
      }
    },

    includeAllLocation($event) {
      const checked = $event.target.checked;
      if (checked) {
        this.locationShowall = true;
        this.locationTags = [];
      } else {
        this.locationShowall = false;
      }
    },

    includeDepartmentData(value, $event) {
      this.departmentShowall = false;
      if (value === 'all') {
        this.locationTags = [];
      } else {
        const index = this.categoryTags?.findIndex((v) => v === value);
        const checked = $event.target.checked;
        if (checked && index < 0) {
          this.categoryTags.push(value);
        }
        if (!checked && index >= 0) {
          this.categoryTags.splice(index, 1);
        }
      }
    },

    includeLocationData(value, $event) {
      this.locationShowall = false;
      if (value === 'all') {
        this.locationTags = [];
      } else {
        const index = this.locationTags?.findIndex((v) => v === value);
        const checked = $event.target.checked;
        if (checked && index < 0) {
          this.locationTags.push(value);
        }
        if (!checked && index >= 0) {
          this.locationTags.splice(index, 1);
        }
      }
    },

    refreshData() {
      let arr = [];
      let categories = this.categoryTags;
      let locations = this.locationTags;
      if (this.departmentShowall) {
        categories = this.allDepartments;
      }
      if (this.locationShowall) {
        locations = this.allLocations;
      }
      for (let i = 0; i < categories?.length; i++) {
        const dep = categories[i];
        for (let j = 0; j < locations?.length; j++) {
          const loc = locations[j];
          const newArray = this.info.filter(function (el) {
            return el.department === dep && el.location?.name === loc;
          });
          arr = arr.concat(newArray);
        }
      }
      this.allCareers = arr;
    },

    loadData() {
      const allDep = [];
      const allloc = [];
      for (let i = 0; i < this.info.length; i++) {
        const element = this.info[i];
        allDep.push(element?.department);
        allloc.push(element?.location?.name);
      }

      function removeDuplicates(arr) {
        return arr.filter((item, index) => arr.indexOf(item) === index);
      }

      this.allDepartments = removeDuplicates(allDep.sort());
      this.allLocations = removeDuplicates(allloc.sort());
      this.categoryTags = [];
      this.locationTags = [];
    },

    morePost() {
      this.filteredPost = this.allCareers?.slice(0, this.filteredPost?.length + 10);
    },

   
  },
});
</script>
<style scoped>

@media (max-width: 767px) {
  .mobile-flex {
    display: flex;
    flex-wrap: wrap;
  }
  .mobile-flex div {
    flex: 0 0 33.33%;
    max-width: 33.33%;
  }
}

.load-more-arrow-icon svg {
  transform: rotate(90deg);
  -webkit-transform: rotate(90deg);
}
.load-more-arrow-icon:hover .load-more-text {
  color: var(--color-brandBlue);
}
.load-more-arrow-icon:hover .svg-down {
  background: var(--color-brandBlue);
}
.svg-down {
  height: 30px;
  width: 30px;
  background-color: rgba(1, 14, 42, 0.04);
  margin-left: 5px;
  border-radius: 50%;
  justify-content: center;
  display: flex;
  align-items: center;
}
.event-heading {
  margin: 0% 12px 80px 4%;
  height: 50px;
  font-style: normal;
  font-weight: 900;
  font-size: 50px;
  line-height: 100%;
  letter-spacing: -0.01em;
  color: #0e142c;
  opacity: 1 !important;
}
.categories {
  display: inline-block;
  font-style: normal;
  font-weight: 700;
  font-size: 16px;
  line-height: 140%;
  text-align: center;
  color: #0e142c;
  padding-bottom: 11px;
  margin-top: 12px;
}
.categories:hover {
  color: blue;
  cursor: pointer;
}

.selected-tag {
  width: 52px;
  height: 14px;
  font-weight: 900;
  font-size: 14px;
  line-height: 100%;
  color: #18a3d1;
  flex: none;
  order: 0;
  flex-grow: 0;
}

.tags {
  height: 14px;
  font-style: normal;
  font-weight: 500;
  font-size: 14px;
  line-height: 100%;
  letter-spacing: 0.002em;
  color: #0e142c;
  flex: none;
  order: 0;
  flex-grow: 0;
}

.tags-post {
  padding-left: 4%;
  position: relative;
  margin-top: -27px;
  border: 1px;
  z-index: 3;
  border-radius: 36px;
}

.load-more-text {
  font-weight: 700;
  font-size: 14px;
  line-height: 100%;
  color: #0e142c;
}

.load-more-arrow-icon {
  margin-top: 10px;
  display: flex;
  align-items: center;
  cursor: pointer;
  margin-left: 9px;
  @media (max-width: 768px) {
    margin-top: 0.1px;
  }
}
@include breakpoint('medium') {
  .filter-container,
  .event-heading {
    padding-left: 100px;
  }
}
.whyJoinText{
  font-family: 'Moderat';
font-style: normal;
font-weight: 700;
font-size: 14px;
line-height: 100%;
letter-spacing: 0.01em;
font-feature-settings: 'tnum' on, 'lnum' on;
color: #1FD9FE;
}

.midTitle2{
  font-family: 'Moderat';
font-style: normal;
font-weight: 900;
font-size: 50px;
line-height: 100%;
letter-spacing: -0.01em;
color: #FFFFFF;
}
.companyBenefit{
  font-family: 'Moderat';
font-style: normal;
font-weight: 400;
font-size: 19px;
line-height: 150%;
font-feature-settings: 'tnum' on, 'lnum' on, 'case' on, 'zero' on;
color: rgba(255, 255, 255, 0.8);
}

</style>
