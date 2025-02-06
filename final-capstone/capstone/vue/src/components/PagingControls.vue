<template>
  <section id="paging-section">
    <div>
      <button v-show="paging.offset > 0" @click="prevPage">Prev</button>
      <button v-show="paging.offset + paging.resultsPerPage.selectedOption < paging.totalCount"
        @click="nextPage">Next</button>
    </div>

    <div id="results-per-page">
      Results per page:
      <ul>
        <li :class="{ 'selected': opt == paging.resultsPerPage.selectedOption }"
          v-for="opt in paging.resultsPerPage.options" :key="`results-per-page-opt-${opt}`"
          @click="updateResultsPerPage(opt)">
          {{ opt }}
        </li>
      </ul>
    </div>
  </section>
</template>

<script>
export default {
  created() {
    if (this.$route.query.resultsPerPage != null) {
      this.paging.resultsPerPage.selectedOption = Number(this.$route.query.resultsPerPage);
    }

    if (this.$route.query.offset != null) {
      this.paging.offset = Number(this.$route.query.offset);
    }

    this.getMore();
  },

  data() {
    return {
      paging: {
        offset: 0,

        resultsPerPage: {
          options: [10, 20, 50],
          selectedOption: 20
        },

        totalCount: 0
      },
    }
  },

  emits: ['paging-data-changed'],

  methods: {
    getMore() {
      this.$emit('paging-data-changed', this.paging);
    },

    prevPage() {
      this.paging.offset -= this.paging.resultsPerPage.selectedOption;

      if (this.paging.offset < 0) {
        this.paging.offset = 0;
      }

      this.getMore();
      this.updateUrl();
    },

    nextPage() {
      this.paging.offset += this.paging.resultsPerPage.selectedOption;
      this.getMore();
      this.updateUrl();
    },

    updateResultsPerPage(val) {
      this.paging.resultsPerPage.selectedOption = val;
      this.getMore();
      this.updateUrl();
    },

    updateUrl() {
      const queryObj = {
        ...this.$route.query,
        resultsPerPage: this.paging.resultsPerPage.selectedOption,
        offset: this.paging.offset
      };

      this.$router.replace({ name: this.$route.name, query: queryObj });
    }
  }
}
</script>

<style scoped>
#paging-section {
  border-bottom: 1px solid var(--color-poke-blue);
  padding-bottom: .25rem;
  margin-bottom: 1rem;
  ;

  display: flex;
  justify-content: space-between;
  align-items: baseline;
}

#results-per-page {
  display: flex;
}

#results-per-page ul {
  display: flex;
  flex-grow: 1;
  gap: 4px;
}

#results-per-page li {
  border: 2px solid var(--color-poke-blue);
  border-radius: 10px;
  cursor: pointer;
  list-style-type: none;
  text-align: center;
  width: 35px;
}

#results-per-page li.selected {
  background-color: var(--color-poke-blue);
  color: var(--color-poke-yellow);
  cursor: default;
}
</style>