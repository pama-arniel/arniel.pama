<template>
  <div>
    <ProjectDetailPopup
      :project="selectedProjectCard"
      :showModal="showModal"
      @close-modal="closeModal"
    />

    <!-- search bar component -->
    <div class="flex items-center justify-center pb-6">
      <div class="box-wrapper text-base text-white">
        <div
          class="rounded-lg flex items-center w-full p-3 shadow-sm bg-black bg-opacity-80 border border-gray-300"
        >
          <button class="outline-none focus:outline-none">
            <svg
              class="w-5 text-gray-600 h-5 cursor-pointer"
              fill="none"
              stroke-linecap="round"
              stroke-linejoin="round"
              stroke-width="2"
              stroke="currentColor"
              viewBox="0 0 24 24"
            >
              <path d="M21 21l-6-6m2-5a7 7 0 11-14 0 7 7 0 0114 0z"></path>
            </svg>
          </button>
          <input
            @input="debounceSearch"
            type="search"
            placeholder="search related terms"
            x-model="q"
            class="w-full pl-4 text-sm outline-none focus:outline-none bg-transparent"
          />
          <div class="select font-medium">
            <select
              v-model="chosenTag"
              x-model="image_type"
              class="text-sm outline-none focus:outline-none bg-black"
            >
              <option
                v-for="(tag, i) in currTagsList"
                :key="refString + 'tag-' + i"
                :value="tag"
                selected
              >
                {{ tag }}
              </option>
            </select>
          </div>
        </div>
      </div>
    </div>

    <!-- Slideshow container -->
    <div v-if="typing" class="flex flex-col text-center w-full">
      <p class="lg:w-2/3 mx-auto leading-relaxed text-base text-gray-500">
        {{ this.typing }}
      </p>
    </div>
    <div
      v-else-if="currNumOfGroups > 0"
      :id="slideShowContainerId"
      class="slideshow-container"
    >
      <div v-if="refString == 'projects'">
        <div
          v-for="i in numOfProjectsGroups"
          :key="'project-group-' + i"
          :class="classSlidesValue"
        >
          <ProjectsCards
            :projects="
              filteredProjectsList.slice(
                i * PROJECTS_PER_GROUP - PROJECTS_PER_GROUP,
                i * PROJECTS_PER_GROUP
              )
            "
            @project-card-clicked="handleClickedProjectCard"
          />
        </div>
      </div>
      <div v-if="refString == 'articles'">
        <div
          v-for="i in numOfArticlesGroups"
          :key="'article-group-' + i"
          :class="classSlidesValue"
        >
          <ArticlesCards
            :articles="
              filteredArticlesList.slice(
                i * ARTICLES_PER_GROUP - ARTICLES_PER_GROUP,
                i * ARTICLES_PER_GROUP
              )
            "
            @clicked-learn-more="handleClickedLearnMore"
          />
        </div>
      </div>
      <!-- Next and previous buttons -->
      <a
        :id="`${refString}-prev-button`"
        v-if="currNumOfGroups > 1"
        class="prev"
        @click="plusSlides(-1, `${refString}-prev-button`)"
        >&#10094;</a
      >
      <a
        :id="`${refString}-next-button`"
        v-if="currNumOfGroups > 1"
        class="next"
        @click="plusSlides(1, `${refString}-next-button`)"
        >&#10095;</a
      >
    </div>
    <div v-else class="flex flex-col text-center w-full">
      <p class="lg:w-2/3 mx-auto leading-relaxed text-base text-gray-500">
        Your search did not match any documents.
      </p>
    </div>
    <br />

    <!-- The dots/circles -->
    <div
      v-if="!typing"
      :id="`${refString}-dots-button`"
      style="text-align: center"
    >
      <span
        v-for="i in currNumOfGroups"
        :key="i"
        :class="classDotValue"
        @click="currentSlide(i, `${refString}-dots-button`)"
      ></span>
    </div>
  </div>
</template>

<script>
import ProjectDetailPopup from "./ProjectDetailPopup.vue";
import ProjectsCards from "./ProjectsCards.vue";
import ArticlesCards from "./ArticlesCards.vue";
import articlesJSON from "../assets/articles/articles_list.json";
import projectsJSON from "../assets/projects/projects_list.json";

export default {
  name: "Carousel",
  props: {
    // added to class names to distinguish the carousel section
    refString: {
      type: String,
      required: true,
    },
  },
  components: {
    ProjectDetailPopup,
    ProjectsCards,
    ArticlesCards,
  },
  data() {
    return {
      slideIndex: 1,
      searchKey: "",
      chosenTag: "All",
      typing: "",
      showModal: false,
      selectedProjectCard: null,

      // determine which of the dot buttons or the next-prev buttons are clicked
      clickedTransitionButtonID: "",

      ARTICLES_PER_GROUP: 4,
      PROJECTS_PER_GROUP: 4,

      tagsForProjects: [
        "All",
        "Machine Learning",
        "Web",
        "Mobile",
        "Games",
        "Special Problems",
      ],
      tagsForArticles: [
        "All",
        "Machine Learning",
        "Project Management",
        "Special Problems",
      ],

      filteredArticlesList: articlesJSON.list,
      filteredProjectsList: projectsJSON.list,

      myInterval: null,
    };
  },
  watch: {
    chosenTag(newChosenTag) {
      this.initiateRefilteringOfList(this.searchKey, newChosenTag);
    },

    filteredArticlesList() {
      this.showSlideDefaults();
    },

    filteredProjectsList() {
      this.showSlideDefaults();
    },

    selectedProjectCard(newVal) {
      if (newVal) {
        clearInterval(this.myInterval);
      }
    },
  },
  computed: {
    classSlidesValue() {
      return `${this.refString}-mySlides mySlides fade`;
    },

    classDotValue() {
      return `${this.refString}-dot dot`;
    },

    currTagsList() {
      if (this.refString == "articles") {
        return this.tagsForArticles;
      }
      return this.tagsForProjects;
    },

    currNumOfGroups() {
      if (this.refString == "articles") {
        return this.numOfArticlesGroups;
      }
      return this.numOfProjectsGroups;
    },

    numOfArticlesGroups() {
      return Math.ceil(
        this.filteredArticlesList.length / this.ARTICLES_PER_GROUP
      );
    },

    numOfProjectsGroups() {
      return Math.ceil(
        this.filteredProjectsList.length / this.PROJECTS_PER_GROUP
      );
    },

    slideShowContainerId() {
      return `slideshow-container-${this.refString}`;
    },
  },
  mounted() {
    this.showSlides(this.slideIndex, true);
    // this.myInterval = setInterval(this.nextSlide, 5000);
  },
  beforeUnmount() {
    clearInterval(this.myInterval);
  },
  methods: {
    handleClickedLearnMore() {
      clearInterval(this.myInterval);
    },

    nextSlide() {
      this.slideIndex++;
      this.showSlides(this.slideIndex);
    },

    closeModal() {
      this.showModal = false;
      this.selectedProjectCard = null;
    },

    handleClickedProjectCard(project) {
      this.selectedProjectCard = project;
      this.showModal = true;
    },

    debounceSearch(event) {
      let newSearchKey = event.target.value;
      this.typing = newSearchKey ? "Typing..." : "";
      clearTimeout(this.debounce);
      this.debounce = setTimeout(() => {
        this.typing = "";
        this.searchKey = newSearchKey;

        // filter the new list
        this.initiateRefilteringOfList(newSearchKey, this.chosenTag);
      }, 600);
    },

    initiateRefilteringOfList(searchKey, chosenTag) {
      if (this.refString == "articles") {
        // make opposite filtered array empty (save memory size)
        this.filteredProjectsList = [];

        // filter projects by search and by tag
        this.filteredArticlesList = this.filterListBySearchKeyAndTag(
          articlesJSON.list,
          searchKey,
          chosenTag
        );
      } else if (this.refString == "projects") {
        // make opposite filtered array empty (save memory size)
        this.filteredArticlesList = [];

        // filter projects by search and by tag
        this.filteredProjectsList = this.filterListBySearchKeyAndTag(
          projectsJSON.list,
          searchKey,
          chosenTag
        );
      }
    },

    // determine if an item's key values contain the search term
    filterListBySearchKeyAndTag(origList, searchKey, tag) {
      if (!searchKey && tag == "All") return origList;

      return origList.filter((item) => {
        return this.isItemIncludedInFilter(item, searchKey, tag);
      });
    },

    isItemIncludedInFilter(item, searchKey, tag) {
      let tags = item?.tags?.join(",");
      let tagRegex = new RegExp(tag, "gi");

      if (tags.match(tagRegex)) {
        if (!searchKey) {
          return true;
        } else {
          let keysToSkip = [
            "company_link",
            "website",
            "project_repo",
            "tags",
            "href",
            "figma_designs",
          ];

          // iterate the key-value pairs of the item and
          // determine if the search term is found in one of them
          for (const [currKey, currValue] of Object.entries(item)) {
            // skip keys that should not be included in the search
            if (keysToSkip.includes(currKey)) continue;

            if (currValue) {
              let completeValue = Array.isArray(currValue)
                ? currValue?.join(",")
                : currValue;
              let keyRegex = new RegExp(searchKey, "gi");

              // if found in array, break loop and return true
              if (completeValue.match(keyRegex)) {
                return true;
              }
            }
          }

          return false;
        }
      } else {
        // early return; return false when tag is not found
        return false;
      }
    },

    showSlideDefaults() {
      this.$nextTick(() => {
        this.slideIndex = 1;
        this.showSlides(this.slideIndex);
      });
    },

    // Next/previous controls
    plusSlides(n, clickedButton) {
      this.clickedTransitionButtonID = clickedButton;
      this.slideIndex += n;
      this.showSlides(this.slideIndex);
      clearInterval(this.myInterval);
    },

    // Thumbnail image controls
    currentSlide(n, clickedButton) {
      this.clickedTransitionButtonID = clickedButton;
      this.slideIndex = n;
      this.showSlides(this.slideIndex);
      clearInterval(this.myInterval);
    },

    scrollSearchBarIntoView() {
      let elmnt = document.getElementById(this.slideShowContainerId);
      elmnt.scrollIntoView({ block: "center" });
    },

    showSlides(n, calledOnMount = false) {
      // don't show slides if no results
      if (this.currNumOfGroups <= 0) return;

      let i;
      let slides = document.getElementsByClassName(this.classSlidesValue);
      let dots = document.getElementsByClassName(this.classDotValue);

      if (n > slides.length) {
        this.slideIndex = 1;
      }

      if (n < 1) {
        this.slideIndex = slides.length;
      }

      for (i = 0; i < slides.length; i++) {
        slides[i].style.display = "none";
      }

      for (i = 0; i < dots.length; i++) {
        dots[i].className = dots[i].className.replace(" active", "");
      }

      if (slides[this.slideIndex - 1]) {
        slides[this.slideIndex - 1].style.display = "block";
      }

      if (dots[this.slideIndex - 1]) {
        dots[this.slideIndex - 1].className += " active";
      }

      // --- deprecated ---
      // scroll to the dots or prev-next area so they are always
      // visible to the user if they were the last buttons clicked
      // if(this.clickedTransitionButtonID){
      //   let elmnt = document.getElementById(this.clickedTransitionButtonID);
      //   elmnt.scrollIntoView({block: "center"});
      //   this.clickedTransitionButtonID = "";
      // }

      // only scroll into view if myInterval is null (meaning the slide event is from the user) and not called on mount
      if (!this.myInterval && !calledOnMount) {
        this.scrollSearchBarIntoView();
      }
    },
  },
};
</script>

<style scoped>
* {
  box-sizing: border-box;
}

/* Slideshow container */
.slideshow-container {
  position: relative;
  margin: auto;
}

/* Hide the images by default */
.mySlides {
  display: none;
}

/* Next & previous buttons */
.prev,
.next {
  cursor: pointer;
  position: absolute;
  /* top: 50%; */
  width: auto;
  /* margin-top: -22px; */
  padding: 10px 18px;
  color: #8a00ff;
  text-shadow: 1px 3px 12px #ffffff42;
  box-shadow: -1px -3px 6px #bf00ff32, 1px 3px 6px #8a00ff32;
  font-weight: bold;
  font-size: 18px;
  transition: 0.6s ease;
  border-radius: 50%;
  user-select: none;
}

/* Position the "next button" to the right */
.next {
  right: 0;
  border-radius: 50%;
}

/* On hover, add a black background color with a little bit see-through */
.prev:hover,
.next:hover {
  background-color: rgba(0, 0, 0, 0.8);
}

/* The dots/bullets/indicators */
.dot {
  cursor: pointer;
  height: 15px;
  width: 15px;
  margin: 0 2px;
  background-color: #bbb;
  border-radius: 50%;
  display: inline-block;
  transition: background-color 0.6s ease;
}

.active,
.dot:hover {
  background-color: rgb(138, 0, 255, 1);
}

/* Fading animation */
.fade {
  -webkit-animation-name: fade;
  -webkit-animation-duration: 1.5s;
  animation-name: fade;
  animation-duration: 1.5s;
}

@-webkit-keyframes fade {
  from {
    opacity: 0.4;
  }
  to {
    opacity: 1;
  }
}

@keyframes fade {
  from {
    opacity: 0.4;
  }
  to {
    opacity: 1;
  }
}
</style>
