<template>
  <section id="projects">
    <AnimateOnVisible name="fadeDown" :duration="1">
		<Title
		class="title"
		:title="content.metadata.title"
		:description="content.metadata.description"
		/>
    </AnimateOnVisible>

    <div class="container-fluid center-block text-center">
      <button class="filter-btn" @click="filterProjects('')">All</button>
      <button class="filter-btn" @click="filterProjects('web3')">Web3</button>
      <button class="filter-btn" @click="filterProjects('game|interactive')">Games/Interactive</button>
      <button class="filter-btn" @click="filterProjects('vr')">VR</button>
      <button class="filter-btn" @click="filterProjects('ar')">AR</button>
      <button class="filter-btn" @click="filterProjects('web|software')">Web/Software</button>
    </div>
    <div class="container-fluid center-block">
      <article class="content text-center">
        <AnimateOnVisible class="timeline mx-auto" v-for="(post, index) in content.metadata.items" :key="index" name="fadeLeft" :duration="0.5">
          <template v-if="checkCategoryMatch(post.tag)">
            <vue-timeline-update
              :date="new Date(post.date)"
              :title="post.title"
              :description="post.content"
              :thumbnail="getImgUrl(post.image)"
              :color="post.color"
              :category="post.tag"
              icon="code"
            />
          </template>
        </AnimateOnVisible>
      </article>
    </div>
  </section>
</template>

<script>
import Title from "./Title.vue";

export default {
  name: "Projects",
  props: ['content'],
  components: {
    Title
  },
  methods: {
    getImgUrl(img) {
      if(img == undefined || img == "")
        return ""
      return require('../assets/img/projects/'+img)
    },
    filterProjects(category) {
      this.selectedCategory = category;
    },
    checkCategoryMatch(tag) {
      if(this.selectedCategory == '') {
        return true
      }
      let lowerCaseTag = tag.toString().toLowerCase()
      const category = this.selectedCategory.toString().toLowerCase()
      const split = category.split("|")
      for (let index = 0; index < split.length; index++) {
        const element = split[index];
        if(this.selectedCategory != 'web3') {
          if(lowerCaseTag.includes('web3')) {
            return false
          }
        }
        if(this.selectedCategory == 'ar') {
          if(lowerCaseTag.includes('software')) {
            return false
          }
        }
        if(lowerCaseTag.includes(element)) {
          return true
        }
      }
      return false
    }
  },
  data() {
    return {
      selectedCategory: ''
    };
  },
};
</script>

<style scoped lang="scss">
@import "@/styles/constants.scss";

$linear: map-get($colors, dark);

#projects {
  background-color: lighten(map-get($colors, primary), 5%);

}

.title {
  color: map-get($colors, light);
}

/deep/ .text-wrapper {
  &:after {
    border-bottom: 1px solid map-get($colors, dark);
  }
}

.filter-btn {
  background-color: #11c482;
  border: none;
  color: white;
  padding: 10px;
  text-align: center;
  text-decoration: none;
  display: inline-block;
  font-size: 14px;
  margin: 12px 2px;
  border-radius: 2px;
}

article .inner {
    position: relative;
    display: inline-block;
    vertical-align: middle;
    z-index: 1;
}

.content {
    color: map-get($colors, light);
    margin-top: 30px;
    header {
        height: 100%;
        width: 70%;
    }
    h1{
        font-size: 3rem;
    }
}

.vertical-center {
    display: flex;
    align-items: center;
}

h1 {
    margin-top: 10px;
    margin-bottom: 20px;
}
</style>
