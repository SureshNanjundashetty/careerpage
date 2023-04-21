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
     info: [
        {
          name: 'Assistant Controller',
          department: 'Finance',
          email: 'vastdata.21.637@applynow.io',
          location: {
            name: 'Tel Aviv',
            country: 'IL',
            city: 'Tel Aviv-Yafo',
            state: 'Tel Aviv District',
            postal_code: '',
            street_name: '97 Rokach Boulevard',
            arrival_instructions: '<p>https://www.google.com/maps?q=32.1039507,34.8085298&amp;z=17&amp;hl=en<br/></p>',
            street_number: null,
            timezone: 'Asia/Jerusalem',
            location_uid: 'E2.101',
            is_remote: false,
          },
          url_comeet_hosted_page: 'https://www.comeet.com/jobs/vastdata/43.001/assistant-controller/21.637',
          url_active_page: 'https://vastdata.com/careers/co/tel-aviv/21.637/assistant-controller/all',
          employment_type: 'Full-time',
          experience_level: null,
          uid: '21.637',
          internal_use_custom_id: null,
          url_detected_page: 'https://vastdata.com/careers/co/tel-aviv/21.637/assistant-controller/all',
          picture_url: null,
          time_updated: '2023-02-21T19:14:33Z',
          company_name: 'VAST Data',
          workplace_type: 'On-Site',
          position_url: 'https://www.comeet.co/careers-api/2.0/company/43.001/positions/21.637?token=bc1qre8jdw2azrg6tf49wmp652w00xltddxmpk98xp',
        },
        {
          name: 'Backend Developer – Python Expert',
          department: 'Engineering',
          email: 'vastdata.D8.700@applynow.io',
          location: {
            name: 'Tel Aviv',
            country: 'IL',
            city: 'Tel Aviv-Yafo',
            state: 'Tel Aviv District',
            postal_code: '',
            street_name: '97 Rokach Boulevard',
            arrival_instructions: '<p>https://www.google.com/maps?q=32.1039507,34.8085298&amp;z=17&amp;hl=en<br/></p>',
            street_number: null,
            timezone: 'Asia/Jerusalem',
            location_uid: 'E2.101',
            is_remote: false,
          },
          url_comeet_hosted_page: 'https://www.comeet.com/jobs/vastdata/43.001/backend-developer--python-expert/D8.700',
          url_active_page: 'https://vastdata.com/careers/co/tel-aviv/D8.700/backend-developer-python-expert/all/',
          employment_type: 'Full-time',
          experience_level: 'Senior',
          uid: 'D8.700',
          internal_use_custom_id: null,
          url_detected_page: 'https://vastdata.com/careers/co/tel-aviv/D8.700/backend-developer-python-expert/all/',
          picture_url: 'https://comeet-euw-app.s3.amazonaws.com/833/9835fcdd5586ca80e003f7c99298f9cdca556be0',
          time_updated: '2023-02-20T19:01:35Z',
          company_name: 'VAST Data',
          workplace_type: 'On-Site',
          position_url: 'https://www.comeet.co/careers-api/2.0/company/43.001/positions/D8.700?token=bc1qre8jdw2azrg6tf49wmp652w00xltddxmpk98xp',
        },
        {
          name: 'BI Developer',
          department: 'Engineering',
          email: 'vastdata.AD.721@applynow.io',
          location: {
            name: 'Tel Aviv',
            country: 'IL',
            city: 'Tel Aviv-Yafo',
            state: 'Tel Aviv District',
            postal_code: '',
            street_name: '97 Rokach Boulevard',
            arrival_instructions: '<p>https://www.google.com/maps?q=32.1039507,34.8085298&amp;z=17&amp;hl=en<br/></p>',
            street_number: null,
            timezone: 'Asia/Jerusalem',
            location_uid: 'E2.101',
            is_remote: false,
          },
          url_comeet_hosted_page: 'https://www.comeet.com/jobs/vastdata/43.001/bi-developer/AD.721',
          url_active_page: 'https://vastdata.com/careers/co/tel-aviv/AD.721/bi-developer/all',
          employment_type: null,
          experience_level: null,
          uid: 'AD.721',
          internal_use_custom_id: null,
          url_detected_page: 'https://vastdata.com/careers/co/tel-aviv/AD.721/bi-developer/all',
          picture_url: null,
          time_updated: '2023-02-21T23:52:15Z',
          company_name: 'VAST Data',
          workplace_type: 'On-Site',
          position_url: 'https://www.comeet.co/careers-api/2.0/company/43.001/positions/AD.721?token=bc1qre8jdw2azrg6tf49wmp652w00xltddxmpk98xp',
        },
        {
          name: 'Channel Manager -  Southern European',
          department: 'Sales',
          email: 'vastdata.AE.B28@applynow.io',
          location: {
            name: 'France',
            country: '',
            city: null,
            state: null,
            postal_code: null,
            street_name: null,
            arrival_instructions: null,
            street_number: null,
            timezone: 'Europe/Paris',
            location_uid: '40.300',
            is_remote: false,
          },
          url_comeet_hosted_page: 'https://www.comeet.com/jobs/vastdata/43.001/channel-manager----southern-european/AE.B28',
          url_active_page: 'https://vastdata.com/careers/co/france/AE.B28/channel-manager-southern-european/all',
          employment_type: 'Full-time',
          experience_level: 'Management',
          uid: 'AE.B28',
          internal_use_custom_id: null,
          url_detected_page: 'https://vastdata.com/careers/co/france/AE.B28/channel-manager-southern-european/all',
          picture_url: null,
          time_updated: '2023-02-21T16:15:33Z',
          company_name: 'VAST Data',
          workplace_type: 'On-Site',
          position_url: 'https://www.comeet.co/careers-api/2.0/company/43.001/positions/AE.B28?token=bc1qre8jdw2azrg6tf49wmp652w00xltddxmpk98xp',
        },
        {
          name: 'Channel Manager DACH',
          department: 'Sales',
          email: 'vastdata.54.62D@applynow.io',
          location: {
            name: 'Germany',
            country: 'DE',
            city: null,
            state: null,
            postal_code: null,
            street_name: null,
            arrival_instructions: null,
            street_number: null,
            timezone: 'Europe/Paris',
            location_uid: '5B.107',
            is_remote: true,
          },
          url_comeet_hosted_page: 'https://www.comeet.com/jobs/vastdata/43.001/channel-manager-dach/54.62D',
          url_active_page: 'https://vastdata.com/careers/co/germany/54.62D/channel-manager-dach/all/',
          employment_type: 'Full-time',
          experience_level: 'Intermediate',
          uid: '54.62D',
          internal_use_custom_id: null,
          url_detected_page: 'https://vastdata.com/careers/co/germany/54.62D/channel-manager-dach/all/',
          picture_url: null,
          time_updated: '2023-02-18T12:11:16Z',
          company_name: 'VAST Data',
          workplace_type: 'Remote',
          position_url: 'https://www.comeet.co/careers-api/2.0/company/43.001/positions/54.62D?token=bc1qre8jdw2azrg6tf49wmp652w00xltddxmpk98xp',
        },
        {
          name: 'Channel Manager- Northern Europe',
          department: 'Sales',
          email: 'vastdata.0C.43F@applynow.io',
          location: {
            name: 'UK',
            country: 'GB',
            city: 'UK',
            state: null,
            postal_code: null,
            street_name: null,
            arrival_instructions: '',
            street_number: null,
            timezone: 'Europe/London',
            location_uid: '6F.10C',
            is_remote: true,
          },
          url_comeet_hosted_page: 'https://www.comeet.com/jobs/vastdata/43.001/channel-manager--northern-europe/0C.43F',
          url_active_page: 'https://vastdata.com/careers/co/uk/0C.43F/channel-manager-northern-europe/all/',
          employment_type: 'Full-time',
          experience_level: 'Intermediate',
          uid: '0C.43F',
          internal_use_custom_id: null,
          url_detected_page: 'https://vastdata.com/careers/co/uk/0C.43F/channel-manager-northern-europe/all/',
          picture_url: null,
          time_updated: '2023-02-21T10:09:33Z',
          company_name: 'VAST Data',
          workplace_type: 'Remote',
          position_url: 'https://www.comeet.co/careers-api/2.0/company/43.001/positions/0C.43F?token=bc1qre8jdw2azrg6tf49wmp652w00xltddxmpk98xp',
        },
        {
          name: 'Channel Marketing Manager',
          department: 'Marketing',
          email: 'vastdata.4A.E28@applynow.io',
          location: {
            name: 'United States',
            country: 'US',
            city: 'USA',
            state: null,
            postal_code: null,
            street_name: null,
            arrival_instructions: '',
            street_number: null,
            timezone: 'America/New_York',
            location_uid: '10.306',
            is_remote: true,
          },
          url_comeet_hosted_page: 'https://www.comeet.com/jobs/vastdata/43.001/channel-marketing-manager/4A.E28',
          url_active_page: 'https://vastdata.com/careers/co/united-states/4A.E28/channel-marketing-manager/all/',
          employment_type: 'Full-time',
          experience_level: null,
          uid: '4A.E28',
          internal_use_custom_id: null,
          url_detected_page: 'https://vastdata.com/careers/co/united-states/4A.E28/channel-marketing-manager/all/',
          picture_url: null,
          time_updated: '2023-02-21T18:31:43Z',
          company_name: 'VAST Data',
          workplace_type: 'Remote',
          position_url: 'https://www.comeet.co/careers-api/2.0/company/43.001/positions/4A.E28?token=bc1qre8jdw2azrg6tf49wmp652w00xltddxmpk98xp',
        },
        {
          name: 'Corporate Paralegal',
          department: 'Finance',
          email: 'vastdata.04.425@applynow.io',
          location: {
            name: 'East Coast',
            country: 'US',
            city: '',
            state: null,
            postal_code: null,
            street_name: null,
            arrival_instructions: '',
            street_number: null,
            timezone: 'America/New_York',
            location_uid: 'E2.201',
            is_remote: false,
          },
          url_comeet_hosted_page: 'https://www.comeet.com/jobs/vastdata/43.001/corporate-paralegal/04.425',
          url_active_page: 'https://vastdata.com/careers/co/east-coast/04.425/corporate-paralegal/all/',
          employment_type: 'Full-time',
          experience_level: null,
          uid: '04.425',
          internal_use_custom_id: null,
          url_detected_page: 'https://vastdata.com/careers/co/east-coast/04.425/corporate-paralegal/all/',
          picture_url: null,
          time_updated: '2023-02-21T17:43:08Z',
          company_name: 'VAST Data',
          workplace_type: 'On-Site',
          position_url: 'https://www.comeet.co/careers-api/2.0/company/43.001/positions/04.425?token=bc1qre8jdw2azrg6tf49wmp652w00xltddxmpk98xp',
        },
        {
          name: 'Corporate Paralegal',
          department: 'Finance',
          email: 'vastdata.A5.429@applynow.io',
          location: {
            name: 'United States',
            country: 'US',
            city: 'USA',
            state: null,
            postal_code: null,
            street_name: null,
            arrival_instructions: '',
            street_number: null,
            timezone: 'America/New_York',
            location_uid: '10.306',
            is_remote: false,
          },
          url_comeet_hosted_page: 'https://www.comeet.com/jobs/vastdata/43.001/corporate-paralegal/A5.429',
          url_active_page: 'https://vastdata.com/careers/co/united-states/A5.429/corporate-paralegal/all/',
          employment_type: 'Part-time',
          experience_level: null,
          uid: 'A5.429',
          internal_use_custom_id: null,
          url_detected_page: 'https://vastdata.com/careers/co/united-states/A5.429/corporate-paralegal/all/',
          picture_url: null,
          time_updated: '2023-02-21T17:43:54Z',
          company_name: 'VAST Data',
          workplace_type: 'On-Site',
          position_url: 'https://www.comeet.co/careers-api/2.0/company/43.001/positions/A5.429?token=bc1qre8jdw2azrg6tf49wmp652w00xltddxmpk98xp',
        },
        {
          name: 'Customer Success Engineer',
          department: 'Customer Support',
          email: 'vastdata.96.61C@applynow.io',
          location: {
            name: 'Tel Aviv',
            country: 'IL',
            city: 'Tel Aviv-Yafo',
            state: 'Tel Aviv District',
            postal_code: '',
            street_name: '97 Rokach Boulevard',
            arrival_instructions: '<p>https://www.google.com/maps?q=32.1039507,34.8085298&amp;z=17&amp;hl=en<br/></p>',
            street_number: null,
            timezone: 'Asia/Jerusalem',
            location_uid: 'E2.101',
            is_remote: false,
          },
          url_comeet_hosted_page: 'https://www.comeet.com/jobs/vastdata/43.001/customer-success-engineer/96.61C',
          url_active_page: 'https://vastdata.com/careers/co/tel-aviv/96.61C/customer-success-engineer/all/',
          employment_type: null,
          experience_level: null,
          uid: '96.61C',
          internal_use_custom_id: null,
          url_detected_page: 'https://vastdata.com/careers/co/tel-aviv/96.61C/customer-success-engineer/all/',
          picture_url: null,
          time_updated: '2023-02-21T02:16:02Z',
          company_name: 'VAST Data',
          workplace_type: 'On-Site',
          position_url: 'https://www.comeet.co/careers-api/2.0/company/43.001/positions/96.61C?token=bc1qre8jdw2azrg6tf49wmp652w00xltddxmpk98xp',
        },
        {
          name: 'Customer Success Engineer - Germany',
          department: 'Customer Support',
          email: 'vastdata.26.53C@applynow.io',
          location: {
            name: 'Germany',
            country: 'DE',
            city: null,
            state: null,
            postal_code: null,
            street_name: null,
            arrival_instructions: null,
            street_number: null,
            timezone: 'Europe/Paris',
            location_uid: '5B.107',
            is_remote: false,
          },
          url_comeet_hosted_page: 'https://www.comeet.com/jobs/vastdata/43.001/customer-success-engineer---germany/26.53C',
          url_active_page: 'https://vastdata.com/careers/co/germany/26.53C/customer-success-engineer-germany/all/',
          employment_type: null,
          experience_level: null,
          uid: '26.53C',
          internal_use_custom_id: null,
          url_detected_page: 'https://vastdata.com/careers/co/germany/26.53C/customer-success-engineer-germany/all/',
          picture_url: null,
          time_updated: '2023-02-14T10:36:09Z',
          company_name: 'VAST Data',
          workplace_type: 'On-Site',
          position_url: 'https://www.comeet.co/careers-api/2.0/company/43.001/positions/26.53C?token=bc1qre8jdw2azrg6tf49wmp652w00xltddxmpk98xp',
        },
        {
          name: 'Customer Success Engineer - Netherlands',
          department: 'Customer Support',
          email: 'vastdata.66.33E@applynow.io',
          location: {
            name: 'Netherlands',
            country: 'NL',
            city: 'Amsterdam',
            state: 'NH',
            postal_code: '',
            street_name: '',
            arrival_instructions: null,
            street_number: '',
            timezone: 'Europe/Amsterdam',
            location_uid: '44.30C',
            is_remote: true,
          },
          url_comeet_hosted_page: 'https://www.comeet.com/jobs/vastdata/43.001/customer-success-engineer---netherlands/66.33E',
          url_active_page: 'https://vastdata.com/careers/co/netherlands/66.33E/customer-success-engineer-netherlands/all/?t=1675874089604',
          employment_type: 'Full-time',
          experience_level: 'Intermediate',
          uid: '66.33E',
          internal_use_custom_id: null,
          url_detected_page: 'https://vastdata.com/careers/co/netherlands/66.33E/customer-success-engineer-netherlands/all/?t=1675874089604',
          picture_url: null,
          time_updated: '2023-02-22T07:11:34Z',
          company_name: 'VAST Data',
          workplace_type: 'Remote',
          position_url: 'https://www.comeet.co/careers-api/2.0/company/43.001/positions/66.33E?token=bc1qre8jdw2azrg6tf49wmp652w00xltddxmpk98xp',
        },
        {
          name: 'Customer Success Engineer -France',
          department: 'Customer Support',
          email: 'vastdata.66.33D@applynow.io',
          location: {
            name: 'France',
            country: '',
            city: null,
            state: null,
            postal_code: null,
            street_name: null,
            arrival_instructions: null,
            street_number: null,
            timezone: 'Europe/Paris',
            location_uid: '40.300',
            is_remote: true,
          },
          url_comeet_hosted_page: 'https://www.comeet.com/jobs/vastdata/43.001/customer-success-engineer--france/66.33D',
          url_active_page: 'https://vastdata.com/careers/co/france/66.33D/customer-success-engineer-france/all',
          employment_type: 'Full-time',
          experience_level: 'Intermediate',
          uid: '66.33D',
          internal_use_custom_id: null,
          url_detected_page: 'https://vastdata.com/careers/co/france/66.33D/customer-success-engineer-france/all',
          picture_url: null,
          time_updated: '2023-02-17T10:28:02Z',
          company_name: 'VAST Data',
          workplace_type: 'Remote',
          position_url: 'https://www.comeet.co/careers-api/2.0/company/43.001/positions/66.33D?token=bc1qre8jdw2azrg6tf49wmp652w00xltddxmpk98xp',
        },
        {
          name: 'Customer Success Engineer- Prague',
          department: 'Customer Support',
          email: 'vastdata.97.33B@applynow.io',
          location: {
            name: 'Prague',
            country: '',
            city: null,
            state: null,
            postal_code: null,
            street_name: null,
            arrival_instructions: null,
            street_number: null,
            timezone: 'Europe/Amsterdam',
            location_uid: '59.304',
            is_remote: true,
          },
          url_comeet_hosted_page: 'https://www.comeet.com/jobs/vastdata/43.001/customer-success-engineer--prague/97.33B',
          url_active_page: 'https://vastdata.com/careers/co/prague/97.33B/customer-success-engineer-prague/all',
          employment_type: 'Full-time',
          experience_level: 'Intermediate',
          uid: '97.33B',
          internal_use_custom_id: null,
          url_detected_page: 'https://vastdata.com/careers/co/prague/97.33B/customer-success-engineer-prague/all',
          picture_url: null,
          time_updated: '2023-01-31T07:00:45Z',
          company_name: 'VAST Data',
          workplace_type: 'Remote',
          position_url: 'https://www.comeet.co/careers-api/2.0/company/43.001/positions/97.33B?token=bc1qre8jdw2azrg6tf49wmp652w00xltddxmpk98xp',
        },
        {
          name: 'Customer Success Engineer- South Korea',
          department: 'Customer Support',
          email: 'vastdata.76.330@applynow.io',
          location: {
            name: 'South Korea',
            country: 'KR',
            city: 'Seoul',
            state: null,
            postal_code: null,
            street_name: null,
            arrival_instructions: '',
            street_number: null,
            timezone: 'Asia/Seoul',
            location_uid: '30.208',
            is_remote: false,
          },
          url_comeet_hosted_page: 'https://www.comeet.com/jobs/vastdata/43.001/customer-success-engineer--south-korea/76.330',
          url_active_page: 'https://vastdata.com/careers/co/south-korea/76.330/customer-success-engineer-south-korea/all/',
          employment_type: 'Full-time',
          experience_level: 'Intermediate',
          uid: '76.330',
          internal_use_custom_id: null,
          url_detected_page: 'https://vastdata.com/careers/co/south-korea/76.330/customer-success-engineer-south-korea/all/',
          picture_url: null,
          time_updated: '2023-02-20T00:38:56Z',
          company_name: 'VAST Data',
          workplace_type: 'On-Site',
          position_url: 'https://www.comeet.co/careers-api/2.0/company/43.001/positions/76.330?token=bc1qre8jdw2azrg6tf49wmp652w00xltddxmpk98xp',
        },
        {
          name: 'Customer Success Manager',
          department: 'Customer Support',
          email: 'vastdata.04.33F@applynow.io',
          location: {
            name: 'United States',
            country: 'US',
            city: 'USA',
            state: null,
            postal_code: null,
            street_name: null,
            arrival_instructions: '',
            street_number: null,
            timezone: 'America/New_York',
            location_uid: '10.306',
            is_remote: true,
          },
          url_comeet_hosted_page: 'https://www.comeet.com/jobs/vastdata/43.001/customer-success-manager/04.33F',
          url_active_page: 'https://vastdata.com/careers/co/united-states/04.33F/customer-success-manager/all',
          employment_type: 'Full-time',
          experience_level: null,
          uid: '04.33F',
          internal_use_custom_id: null,
          url_detected_page: 'https://vastdata.com/careers/co/united-states/04.33F/customer-success-manager/all',
          picture_url: null,
          time_updated: '2023-02-22T07:17:37Z',
          company_name: 'VAST Data',
          workplace_type: 'Remote',
          position_url: 'https://www.comeet.co/careers-api/2.0/company/43.001/positions/04.33F?token=bc1qre8jdw2azrg6tf49wmp652w00xltddxmpk98xp',
        },
        {
          name: 'Customer success Manager',
          department: 'Customer Support',
          email: 'vastdata.76.334@applynow.io',
          location: {
            name: 'UK',
            country: 'GB',
            city: 'UK',
            state: null,
            postal_code: null,
            street_name: null,
            arrival_instructions: '',
            street_number: null,
            timezone: 'Europe/London',
            location_uid: '6F.10C',
            is_remote: false,
          },
          url_comeet_hosted_page: 'https://www.comeet.com/jobs/vastdata/43.001/customer-success-manager/76.334',
          url_active_page: 'https://vastdata.com/careers/co/uk/76.334/customer-success-manager/all/',
          employment_type: 'Full-time',
          experience_level: 'Intermediate',
          uid: '76.334',
          internal_use_custom_id: null,
          url_detected_page: 'https://vastdata.com/careers/co/uk/76.334/customer-success-manager/all/',
          picture_url: null,
          time_updated: '2023-02-21T12:45:09Z',
          company_name: 'VAST Data',
          workplace_type: 'On-Site',
          position_url: 'https://www.comeet.co/careers-api/2.0/company/43.001/positions/76.334?token=bc1qre8jdw2azrg6tf49wmp652w00xltddxmpk98xp',
        },
        {
          name: 'Customer Success Manager - East Coast',
          department: 'Customer Support',
          email: 'vastdata.BD.530@applynow.io',
          location: {
            name: 'United States',
            country: 'US',
            city: 'USA',
            state: null,
            postal_code: null,
            street_name: null,
            arrival_instructions: '',
            street_number: null,
            timezone: 'America/New_York',
            location_uid: '10.306',
            is_remote: false,
          },
          url_comeet_hosted_page: 'https://www.comeet.com/jobs/vastdata/43.001/customer-success-manager---east-coast/BD.530',
          url_active_page: 'https://vastdata.com/career/customer-success-manager-east-coast/',
          employment_type: 'Full-time',
          experience_level: 'Senior',
          uid: 'BD.530',
          internal_use_custom_id: 'R12023',
          url_detected_page: 'https://vastdata.com/career/customer-success-manager-east-coast/',
          picture_url: null,
          time_updated: '2023-02-22T01:47:20Z',
          company_name: 'VAST Data',
          workplace_type: 'On-Site',
          position_url: 'https://www.comeet.co/careers-api/2.0/company/43.001/positions/BD.530?token=bc1qre8jdw2azrg6tf49wmp652w00xltddxmpk98xp',
        },
        {
          name: 'Customer Success Manager - EMEA',
          department: 'Customer Support',
          email: 'vastdata.88.535@applynow.io',
          location: {
            name: 'EMEA',
            country: '',
            city: null,
            state: null,
            postal_code: null,
            street_name: null,
            arrival_instructions: null,
            street_number: null,
            timezone: 'Europe/London',
            location_uid: '85.300',
            is_remote: false,
          },
          url_comeet_hosted_page: 'https://www.comeet.com/jobs/vastdata/43.001/customer-success-manager---emea/88.535',
          url_active_page: 'https://vastdata.com/careers/co/emea/88.535/customer-success-manager-emea/',
          employment_type: 'Full-time',
          experience_level: 'Senior',
          uid: '88.535',
          internal_use_custom_id: 'R392023',
          url_detected_page: 'https://vastdata.com/careers/co/emea/88.535/customer-success-manager-emea/',
          picture_url: null,
          time_updated: '2023-02-21T14:42:53Z',
          company_name: 'VAST Data',
          workplace_type: 'On-Site',
          position_url: 'https://www.comeet.co/careers-api/2.0/company/43.001/positions/88.535?token=bc1qre8jdw2azrg6tf49wmp652w00xltddxmpk98xp',
        }]
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
    info: {
      handler() {
        this.loadData();
        this.refreshData();
      },
      immediate: true,
    },
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
     //this.loadApi();
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
      if(this.info.length>0){
this.loadData().then(()=>this.refreshData())

      }
      
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
