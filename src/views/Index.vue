<template>
  <JobLayout>
       <div class="items-display">
          <h1 class="title">Find Jobs</h1>
          <div class="jobs-card-array">
            <JobCard v-for="job in jobs && jobs.docs" :key="job._id" :job="job"/>
           <div class="no-results" v-if="!loading && jobs?.docs.length <= 0">
            No result(s) found
          </div>

          </div>
       </div>
      <Pagination v-if="jobs?.docs.length > 0 && !loading" :items="jobs" :totalPages="totalPages" @setPage="setPage" />
  </JobLayout>
</template>

<script>
import { mapGetters } from 'vuex'
import JobLayout from '../layout/job.vue'
import JobCard from '../components/JobCard.vue'
import Pagination from '../components/Pagination.vue';
export default {
  name: "IndexPage",
  components: { JobLayout,
  JobCard,
    Pagination
  },

  data() {
    return {
    }
  },

    computed: {
    ...mapGetters({
      jobs: ["allJobs"],
      queries: ["queries"],
      loading:['loadingState']
    }),

    totalPages() {
      return this.jobs && this.jobs.totalPages
    },

    resultPerPage() {
      return this.jobs && this.jobs.limit
    },

    currentPage() {
      return this.jobs && this.jobs.page
    }

  },

  methods: {
    async setPage() {
      await this.$store.dispatch("fetchJobs", this.queries);
    }
  },

 async created() {
    // this.$toast.success("Home Page");
    await this.$store.dispatch("fetchJobs", this.queries);
  },
};
</script>

<style>
 .items-display { margin-top: 60px; }
 .items-display .title {color: rgb(236, 234, 234); margin-bottom: 40px;}
 .items-display .jobs-card-array { display: grid; grid-template-columns: repeat(2, 1fr); }
 .items-display .jobs-card-array .job-card:nth-child(odd) { margin: 0 26px 40px 0; }
 .items-display .jobs-card-array .job-card:nth-child(even) { margin: 0 0 40px 0; }
 .items-display .no-results { color: #fff; }

  /* MEDIA QUERIES */
  @media (max-width: 991px) {
  .items-display .jobs-card-array { display: grid; grid-template-columns: repeat(1, 1fr); }
  .items-display .jobs-card-array .job-card:nth-child(odd) { margin: 0 0px 40px 0; }
  .items-display .jobs-card-array .job-card:nth-child(even) { margin: 0 0 40px 0; }
  }
  @media (max-width: 767px) {
  .items-display .title {font-size: 22px; margin-bottom: 20px;}
  }
  @media (max-width: 480px) {
   .items-display { margin-top: 30px; }

  }
</style>