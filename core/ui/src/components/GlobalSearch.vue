<template>
  <div class="global-search" v-click-outside="clickOutside">
    <cv-text-input
      placeholder="Search..."
      ref="search"
      @input="inputSearch"
      v-model="query"
    />
    <div v-if="showResults" class="search-results">
      <div v-if="!results.length">
        <div class="empty-state">
          <pictogram title="empty state" class="image">
            <ExclamationMark />
          </pictogram>
          <h5 class="title">No results</h5>
          <div class="description">Try changing your search query</div>
        </div>
      </div>
      <cv-structured-list v-else>
        <template slot="items">
          <cv-structured-list-item
            v-for="(result, index) in results"
            :key="index"
          >
            <cv-structured-list-data>
              <div class="search-result-column">
                <Settings20 class="result-icon" />
                <span>{{ result.name }}</span>
              </div></cv-structured-list-data
            >
            <cv-structured-list-data
              ><div class="search-result-column description">
                <span>{{ result.description }}</span>
              </div></cv-structured-list-data
            >
            <cv-structured-list-data
              ><div class="search-result-column category">
                <span>{{ result.category }}</span>
              </div></cv-structured-list-data
            >
          </cv-structured-list-item>
        </template>
      </cv-structured-list>
    </div>
  </div>
</template>

<script>
import Settings20 from "@carbon/icons-vue/es/settings/20";
import Pictogram from "@/components/Pictogram";
import ExclamationMark from "@/components/pictograms/ExclamationMark";

export default {
  name: "GlobalSearch",
  components: { Settings20, Pictogram, ExclamationMark },
  data() {
    return {
      query: "",
      showResults: false,
      results: [],
      searchFields: ["name", "description", "application", "tags"], ////
      minChars: 1, ////
      maxResults: 10,
      allResults: [
        {
          category: "Applications",
          name: "Firewall",
          description: "Launch Firewall application",
          tags: "gateway,firewall,fw",
        },
        {
          category: "Firewall",
          name: "Create port forward",
          description: "Create port forward in Firewall app",
          tags: "pf,port,port forward",
        },
        {
          category: "System",
          name: "Account",
          description: "Configure your account",
          tags: "user,password",
        },
        {
          category: "System",
          name: "Cluster dashboard",
          description: "Monitor cluster status",
          tags: "monitor,status",
        },
        {
          category: "Applications",
          name: "Nextcloud",
          description: "Content collaboration platform",
          tags: "file,sharing",
        },
        {
          category: "System",
          name: "Cluster nodes",
          description: "Manage and deploy cluster nodes",
          tags: "agent",
        },
      ],
      isClickOutsideEnabled: false,
    };
  },
  mounted() {
    console.log("global search mounted"); ////
    this.focusSearch();

    // prevent glitch: click-outside is incorrectly detected when global search appears
    setTimeout(() => {
      this.isClickOutsideEnabled = true;
    }, 200);
  },
  methods: {
    clickOutside() {
      //// fix, check if it is currently shown
      if (this.isClickOutsideEnabled) {
        this.$emit("closeSearch");
      }
    },
    focusSearch() {
      // focus on search field
      this.$nextTick(() => {
        const searchInput = this.$refs.search;
        searchInput.focus();
      });
    },
    inputSearch() {
      // clean query
      const cleanRegex = /[^a-zA-Z0-9]/g;
      const queryText = this.query.replace(cleanRegex, "");

      if (queryText.length < this.minChars) {
        this.results = [];
        this.showResults = false;
        return;
      }

      // search
      this.results = this.allResults.filter((option) => {
        // compare query text with all search fields of option
        return this.searchFields.some((searchField) => {
          if (option[searchField]) {
            return new RegExp(queryText, "i").test(
              option[searchField].replace(cleanRegex, "")
            );
          } else {
            return false;
          }
        });
      }, this);

      if (this.results.length) {
        // limit maximum number of results
        if (this.results.length > this.maxResults) {
          this.results = this.results.slice(0, this.maxResults);
        }
      }
      this.showResults = true;
    },
  },
};
</script>

<style scoped lang="scss">
@import "../styles/carbon-utils";

.global-search {
  width: 100%;
  background-color: $ui-05;
  color: $ui-01;
}

.global-search .search-results {
  // border-left: 1px solid $interactive-02; ////
  // border-right: 1px solid $interactive-02;
  // border-bottom: 1px solid $interactive-02;
  box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.5);
}

.search-result-column {
  display: flex;
  align-items: center;
}

.search-result-column.description,
.search-result-column.category {
  color: #c6c6c6;
}

.result-icon {
  margin-right: $spacing-03;
}
</style>
