<template>
  <div class="bx--grid bx--grid--full-width">
    <div class="bx--row">
      <div class="bx--col-lg-16">
        <h1 class="page-title">Cluster dashboard</h1>
      </div>
    </div>
    <div class="bx--row">
      <div class="bx--col-md-5">
        <cv-tile :light="true" class="content-tile">
          <h2>Main content</h2>
          <p>This is some tile content</p>
        </cv-tile>
      </div>
      <div class="bx--col-md-3">
        <cv-tile :light="true" class="content-tile">
          <h2>Side content</h2>
          <p>This is some tile content</p>
        </cv-tile>
      </div>
    </div>
    <!-- <div class="bx--row">
      <div class="bx--col-md-4 bx--col-lg-7">7/16</div>
      <div class="bx--col-md-4 bx--offset-lg-1 bx--col-lg-8">8/16</div>
    </div>
    <div class="bx--row">
      <div class="bx--col-md-4 bx--col-lg-4">1/4</div>
      <div class="bx--col-md-4 bx--col-lg-4">1/4</div>
      <div class="bx--col-md-4 bx--col-lg-4">1/4</div>
      <div class="bx--col-md-4 bx--col-lg-4">1/4</div>
    </div> -->
    <div class="bx--row">
      <div class="bx--col-md-4">
        <cv-tile :light="true" class="content-tile">
          <cv-text-input label="Label" v-model="testInput"> </cv-text-input>
          <cv-toggle value="check-test" v-model="testToggle"> </cv-toggle>
          <div class="mg-top-bottom">
            <cv-button kind="" :icon="Flash16" @click="createInfoToast">
              Create info toast
            </cv-button>
          </div>

          <div class="mg-top-bottom">
            <cv-button
              kind="primary"
              :icon="Flash16"
              @click="createSuccessToast"
            >
              Create success toast
            </cv-button>
          </div>

          <div class="mg-top-bottom">
            <cv-button
              kind="secondary"
              :icon="Flash16"
              @click="createWarningToast"
            >
              Create warning toast
            </cv-button>
          </div>

          <div class="mg-top-bottom">
            <cv-button kind="danger" :icon="Flash16" @click="createErrorToast">
              Create error toast
            </cv-button>
          </div>

          <!-- <div class="mg-top-bottom"> ////
            <cv-button
              kind="secondary"
              :icon="Flash16"
              @click="createTaskNotification"
            >
              Create task notification
            </cv-button>
          </div> -->
        </cv-tile>
      </div>
    </div>
    <div class="bx--row">
      <div class="bx--col-md-4">
        <!-- <div class="mg-top-bottom">
          <cv-icon-button
            kind="secondary"
            :icon="Flash16"
            label="Button label"
            tip-position="bottom"
            tip-alignment="center"
          />
        </div> -->

        <!-- <pictogram width="48" height="48" title="gear">
          <gear />
        </pictogram>

        <div>
          <div>Now: {{ now }}</div>
          <div>Now: {{ new Date() | date }}</div>
          <div>Now: {{ $date(new Date()) }}</div>
          <div>
            Relative: {{ formatRelative(subDays(new Date(), 2), new Date()) }}
          </div>
          <div>
            Relative 2:
            {{
              formatDistance(subDays(new Date(), 2), new Date(), {
                addSuffix: true,
              })
            }}
          </div>
        </div> -->

        <!-- <div class="mg-top-bottom">
          <cv-code-snippet :light="true">{{ snippet }}</cv-code-snippet>
        </div> -->

        <!-- <div class="mg-top-bottom">
          <cv-date-picker kind="single" value="01/01/2020"></cv-date-picker>
        </div>

        <div class="mg-top-bottom">
          <cv-tag kind="blue" label="I am a tag"></cv-tag>
        </div> -->
      </div>
    </div>
    <!-- <div class="bx--row">
      <div class="bx--col-md-4">
        <div class="mg-top-bottom">
          <cv-tile :light="true">
            <h1>Test tile</h1>
            <p>This is some tile content</p>
          </cv-tile>
        </div>
      </div>
    </div>
    <div class="bx--row">
      <div class="bx--col-md-6">
        <div class="mg-top-bottom">
          <cv-toggle value="check-1"> </cv-toggle>
        </div>

        <div class="mg-top-bottom">
          <cv-interactive-tooltip alignment="center" direction="right">
            <template v-if="true" slot="label"> Tooltip label </template>
            <template v-if="true" slot="trigger"
              ><Filter16
                class="bx--overflow-menu__icon bx--toolbar-filter-icon"
              />
            </template>
            <template v-if="true" slot="content">
              <p>
                Lorem ipsum dolor sit amet, consectetur adipisicing elit, seed
                do eiusmod tempor incididunt ut labore et dolore magna aliqua.
                Ut enim ad minim veniam, quis nostrud exercitation ullamco
                laboris nisi ut aliquip ex ea commodo consequat.
              </p>
              <button class="bx--button">Clicky one</button>
            </template>
          </cv-interactive-tooltip>
        </div>

        <div class="mg-top-bottom">
          <cv-toast-notification
            v-if="toastVisible"
            kind="info"
            :title="toastTitle"
            :sub-title="toastSubTitle"
            :caption="toastCaption"
            @close="closeToast"
            :low-contrast="lowContrast"
          ></cv-toast-notification>
        </div>
      </div>
      <div class="bx--col-md-4">
        <div class="mg-top-bottom">
          <AreaChart />
        </div>
      </div>
    </div> -->
  </div>
</template>

<script>
// import AreaChart from "@/components/AreaChart"; ////
import Flash16 from "@carbon/icons-vue/es/flash/16";
// import Filter16 from "@carbon/icons-vue/es/filter/16"; ////
import { mapState } from "vuex";
import NotificationService from "@/mixins/notification";
// import Pictogram from "@/components/Pictogram";
// import Gear from "@/components/pictograms/Gear";
import { formatRelative, formatDistance, subDays } from "date-fns";

export default {
  name: "Dashboard",
  components: {},
  mixins: [NotificationService],
  data() {
    return {
      toastVisible: true,
      toastTitle: "Toast title",
      toastSubTitle: "Toast subtitle",
      toastCaption: "Toast caption",
      lowContrast: false,
      snippet: 'printf("A short bit of code.");',
      Flash16, //// use mixin
      formatRelative, //// use mixin
      subDays,
      formatDistance,
      testInput: "",
      testToggle: false,
    };
  },
  computed: {
    ...mapState(["notifications"]),
    ////
    now() {
      return this.$date(new Date());
    },
  },
  beforeRouteEnter(to, from, next) {
    next((vm) => {
      console.log("beforeRouteEnter", to, from); ////
      vm.queryParamsToData(vm, to.query);
    });
  },
  beforeRouteUpdate(to, from, next) {
    console.log("beforeRouteUpdate", to, from); ////
    this.queryParamsToData(this, to.query);
    next();
  },
  watch: {
    testInput: function () {
      if (this.testInput !== this.$route.query.testInput) {
        this.$router.replace({
          query: { ...this.$route.query, testInput: this.testInput },
        });
      }
    },
    testToggle: function () {
      console.log("watch testToggle", this.testToggle); ////

      const booleanQueryTestToggle = this.$route.query.testToggle == "true";

      if (this.testToggle !== booleanQueryTestToggle) {
        this.$router.replace({
          query: { ...this.$route.query, testToggle: this.testToggle },
        });
      }
    },
  },
  methods: {
    queryParamsToData(context, queryParams) {
      console.log("query params", queryParams); ////

      if (queryParams.testInput) {
        context.testInput = queryParams.testInput;
      } else {
        context.testInput = "";
      }

      if (queryParams.testToggle) {
        context.testToggle = queryParams.testToggle === "true";
      } else {
        context.testToggle = false;
      }
    },
    closeToast() {
      console.log("closeToast"); ////
    },
    createWarningToast() {
      const notification = {
        title: "Low disk space",
        text: "You are running out of disk space",
        type: "warning",
        app: "System manager",
      };
      this.createNotification(notification);
    },
    createInfoToast() {
      const notification = {
        title: "Software updates",
        text: "You have 7 new updates",
        type: "info",
        app: "System manager",
      };
      this.createNotification(notification);
    },
    createSuccessToast() {
      const notification = {
        title: "Backup completed",
        text: "Backup data has completed succesfully",
        type: "success",
        app: "Backup manager",
      };
      this.createNotification(notification);
    },
    createErrorToast() {
      const notification = {
        title: "Network error",
        text: "Cannot retrieve cluster info. Check your connection",
        type: "error",
        app: "Cluster manager",
      };
      this.createNotification(notification);
    },
    createTaskNotification() {
      const notification = {
        title: "Installing Nextcloud",
        text: "Configuring...",
        type: "info",
        app: "Cluster manager",
        isTask: true,
        progress: 0,
      };
      this.createNotification(notification);
    },
  },
};
</script>

<style scoped lang="scss"></style>
