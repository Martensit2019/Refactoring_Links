<template>
  <div class="program" :class="{ program_themes_desktop: desktop }">
    <div v-if="false" class="program__success">
      <div class="program__image">
        <div class="program__success_icon">
          <svg
            width="14"
            height="11"
            viewBox="0 0 14 11"
            fill="none"
            xmlns="http://www.w3.org/2000/svg"
          >
            <path
              d="M11.8238 0.350702C12.0657 0.123273 12.3877 -0.00245916 12.7217 3.64524e-05C13.0557 0.00253207 13.3757 0.133061 13.6141 0.364078C13.8525 0.595096 13.9907 0.908535 13.9995 1.23825C14.0084 1.56797 13.8871 1.88819 13.6614 2.13132L6.809 10.5935C6.69117 10.7188 6.54895 10.8194 6.39086 10.8892C6.23277 10.959 6.06205 10.9966 5.8889 10.9998C5.71575 11.0029 5.54373 10.9716 5.38313 10.9076C5.22253 10.8436 5.07665 10.7483 4.9542 10.6274L0.409958 6.14026C0.283408 6.02382 0.181906 5.8834 0.111507 5.72739C0.0411069 5.57137 0.00325191 5.40296 0.000200455 5.23218C-0.002851 5.06141 0.0289636 4.89177 0.0937458 4.7334C0.158528 4.57503 0.254951 4.43117 0.377262 4.31039C0.499573 4.18962 0.645267 4.09441 0.805651 4.03044C0.966036 3.96647 1.13783 3.93506 1.31077 3.93807C1.48372 3.94108 1.65428 3.97846 1.81228 4.04798C1.97028 4.11749 2.11248 4.21772 2.2304 4.34268L5.82664 7.89205L11.7912 0.388011C11.8019 0.374954 11.8134 0.362498 11.8255 0.350702H11.8238Z"
              fill="white"
            />
          </svg>
        </div>
      </div>
    </div>
    <div
      v-tooltip.bottom-start="isWish ? '' : 'Добавить программу в «Избранное»'"
      class="program__flag_block"
      :class="{ program_add_already: isWish }"
    >
      <div v-if="!desktop" class="program__flag" />
      <template v-else>
        <div class="program_flag_inner" @click="addWish(program)">
          <svg
            class="program__flag_img"
            width="14"
            height="20"
            viewBox="0 0 14 20"
          >
            <path
              d="M1.25 18.7057V18.7053V1.24994C1.25 1.05105 1.32901 0.860303 1.46966 0.71966C1.61031 0.579017 1.80108 0.5 2 0.5H12C12.1989 0.5 12.3897 0.579017 12.5303 0.719661C12.671 0.860303 12.75 1.05105 12.75 1.24994V18.7491V18.7495C12.7501 18.8976 12.7063 19.0425 12.6242 19.1659C12.542 19.2892 12.4251 19.3854 12.2883 19.4424C12.1515 19.4993 12.0009 19.5145 11.8555 19.4859C11.71 19.4574 11.5763 19.3864 11.4712 19.2819L7.32774 15.1574L6.97474 14.806L6.622 15.1576L2.53449 19.2324L2.53251 19.2344C2.42783 19.34 2.29415 19.412 2.14844 19.4414C2.00272 19.4708 1.85155 19.4563 1.71412 19.3996L1.52355 19.8619L1.71412 19.3996C1.57669 19.343 1.4592 19.2468 1.37657 19.1232L0.963278 19.3996L1.37657 19.1232C1.29393 18.9997 1.24988 18.8544 1.25 18.7057Z"
              fill="#F1F0F0"
              stroke="#B2B2B2"
            />
          </svg>
        </div>
        <div
          v-show="isWish"
          v-tooltip.bottom-start="'Удалить программу из «Избранного»'"
          class="program__delete_favorite"
          @click="deleteWish(program.id)"
        >
          <svg
            width="6"
            height="6"
            viewBox="0 0 6 6"
            fill="none"
            xmlns="http://www.w3.org/2000/svg"
          >
            <path
              d="M6 0.604286L5.39571 0L3 2.39571L0.604286 0L0 0.604286L2.39571 3L0 5.39571L0.604286 6L3 3.60429L5.39571 6L6 5.39571L3.60429 3L6 0.604286Z"
              fill="#4D4D4D"
            />
          </svg>
        </div>
      </template>
    </div>
    <NuxtLink
      v-if="program.type == 'education_program'"
      :to="{
        name: 'programs-program_id',
        params:
          $route.name === 'programs-program_id'
            ? {}
            : { program_id: program.slug }
      }"
    >
      <div
        class="program__image"
        :style="'background-image: url(' + checkImg(program.image) + ')'"
      >
        <div v-if="progress" class="program__progress_bar">
          <div class="program__progress_bar_indicator" style="width: 33%" />
        </div>
      </div>
    </NuxtLink>
    <NuxtLink
      v-else-if="program.type == 'course'"
      :to="{
        name: 'courses-course',
        params:
          $route.name === 'programs-program_id' ? {} : { course: program.slug }
      }"
    >
      <div
        class="program__image"
        :style="'background-image: url(' + checkImg(program.image) + ')'"
      >
        <div v-if="progress" class="program__progress_bar">
          <div class="program__progress_bar_indicator" style="width: 33%" />
        </div>
      </div>
    </NuxtLink>

    <div class="program__info">
      <NuxtLink
        v-if="program.type == 'education_program'"
        :to="{
          name: 'programs-program_id',
          params:
            $route.name === 'programs-program_id'
              ? {}
              : { program_id: program.slug }
        }"
      >
        <div class="program__title">
          {{ program.name }}
        </div>
      </NuxtLink>
      <NuxtLink
        v-else-if="program.type == 'course'"
        :to="{
          name: 'courses-course',
          params:
            $route.name === 'programs-program_id'
              ? {}
              : { course: program.slug }
        }"
      >
        <div class="program__title">
          {{ program.name }}
        </div>
      </NuxtLink>
      <div class="program__stats program_stats_mb">
        <div
          class="program__stats_like program_stats_mr"
          @click="liked(program.id)"
        >
          <span
            class="material-icons-outlined program_stats_icon_mr program_like_red"
          >
            favorite
          </span>
          {{ likes || 0 }}
        </div>
        <div class="program__stats_views program_stats_mr">
          <span class="material-icons program_stats_icon_mr">
            people
          </span>
          {{ program.users_count }}
        </div>
        <div class="program__stats_duration">
          <span class="material-icons program_stats_icon_mr">
            query_builder
          </span>
          {{ durationParse(program.volume) }}
        </div>
      </div>
      <div v-if="program.tags" class="program__tags">
        <NuxtLink
          v-for="(item, key) in program.tags"
          :key="key"
          :to="{ name: 'tag-tag', params: { tag: item } }"
          class="program__tag"
        >
          #{{ item }}
        </NuxtLink>
      </div>
    </div>
  </div>
</template>

<script>
import mixin from "../../mixin.js";
export default {
  name: "program",
  mixins: [mixin],
  props: {
    program: {
      type: Object,
      default: null
    },
    progress: {
      type: Boolean,
      default: false
    },
    desktop: {
      type: Boolean,
      default: false
    },
    favorite: {
      type: Boolean,
      default: true
    }
  },
  data() {
    return {
      likes: this.program.likes
    };
  },
  computed: {
    isWish() {
      return this.$store.getters["profile/checkWish"](this.program.id);
    }
  },
  methods: {
    addWish(program) {
      if (!this.isWish) {
        this.$store
          .dispatch("profile/ADD_WISHLIST_PROGRAM", program)
          .catch(() => {
            this.$notify({
              group: "error",
              text: "ошибка"
            });
          });
      }
    },
    deleteWish(programId) {
      if (this.isWish) {
        this.$store.dispatch("profile/DELETE_WISHLIST_PROGRAM", programId);
      }
    },
    liked(programId) {
      // this.likes++;
      this.$axios
        .get(`/education-programs/${programId}/like`)
        .then(() => {
          this.likes++;
          this.$store.dispatch("categories/FETCH_PROGRAM", true);
        })
        .catch(() => {
          this.$notify({
            group: "warning",
            text: "Вы уже оценили эту программу"
          });
        });
    }
  }
};
</script>

<style scoped></style>
