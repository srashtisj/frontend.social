<template>
  <div>
    <Loader v-show="loading" />
    <b-container>
      <b-row>
        <b-col md="12">
          <h1>
            <span v-if="type === 'CATCHUP'">Catch up with...</span>
            <span v-else>All about Freelancing</span>
            <button
              v-if="$store.getters.isAdmin"
              @click="onAddVlog"
            >
              + Add
            </button>
          </h1>
          <div
            class="vlogs"
          >
            <VLogStrip
              v-for="vLog in vLogs"
              :key="vLog._id"
              :vlog="vLog"
              :type="type"
              @delete="onDelete($event)"
            />
          </div>
        </b-col>
      </b-row>
    </b-container>
  </div>
</template>

<script>
import VLogStrip from "@/components/VLog/VLogStrip";
import vLogService from "@/services/vlog.service";
import { VLogType } from "@/constants/constants";

export default {
  name: "VLogs",
  components: {
    VLogStrip,
  },
  data() {
    return {
      vLogs: [],
      totalPages: 1,
      pageNo: 1,
      loading: false
    };
  },
  computed: {
    signedInUser() {
      return this.$store.state.signedInUser;
    },
    type() {
      return this.$route.name === 'catchup' ? VLogType.CATCHUP : VLogType.FREELANCING;
    }
  },
  mounted() {
    this.loading = true;
    vLogService.getVLogs(this.type)
      .then(resp => {
        this.vLogs = resp;
        this.loading = false;
      });
  },
  methods: {
    onAddVlog() {
      this.$router.push("/vlog/form/new");
    },
    onDelete(id) {
      this.loading = true;
      vLogService.deleteVLog(id).then(resp => {
        this.vLogs = this.vLogs.filter(vLog => vLog._id !== id);
        this.loading = false;
      });
    }
  }
};
</script>

<style lang="scss" scoped>
.host {
  width: 100%;
}

.vLogs {
  margin: 20px 10px;
  text-align: left;
  width: 100%;
}
</style>
