<template>
  <p v-if="topics">
  <span class="meta">{{meta}}</span>
  <br>
  <ul class="list-unstyled">
    <li v-for="topic in sortedTopics" v-bind:key="topic" class="value">
      <router-link :to="{ name: 'results', params: { data: paramsData(topic) } }">{{topic}}</router-link>
      <br>
      <div v-for="subtopic in getSubtopics(topic)" v-bind:key="subtopic+' - '+topic" class="subtopic">
        <router-link :to="{ name: 'results', params: { data: paramsData(topic, subtopic) } }">{{subtopic}} </router-link>
        <br>
        <span v-for="tag in getTags(subtopic)" v-bind:key="tag+' - '+topic" class="tag">
          <router-link :to="{ name: 'results', params: { data: paramsData(topic, subtopic, tag) } }">{{tag}} </router-link>
        </span>
        <br>
      </div>

    </li>
  </ul>
  </p>
</template>

<script>
import * as Utils from '@/utils'

const qs = require('qs');

export default {
  name: 'topics-element',
  props: [
    'meta',
    'topics',
    'tags'
  ],
  computed: {
    sortedTopics: function() {
      return this.$props.topics.slice().sort(Utils.naturalSort)
    }
  },
  methods: {
    getSubtopics(topic) {
      return [...new Set(this.$props.tags.filter(tag => tag.topic === topic).map(tag => tag.subtopic))];
    },
    getTags: function(subtopic) {
      return this.$props.tags.filter(tag => tag.subtopic === subtopic).map(tag => tag.tag)
    },
    paramsData: function(currentTopic, currentSubtopic, currentTag) {
      return qs.stringify({
        topic: currentTopic,
        subtopics: currentSubtopic ? currentSubtopic : undefined,
        tags: currentTag ? currentTag : undefined
      });
    }
  }
}
</script>

<style scoped lang="scss">
.value {
  margin-bottom: 40px;
  .subtopic {
    font-size: 0.8em;
    margin-left: 20px;
    margin-bottom: 20px;
    .tag {
      background-color: #efefef;
      color: #222;
      padding: 5px;
      margin: 0px 5px 5px 5px;
      font-size: 0.8em;
      font-weight: normal;
    }
  }
}
</style>
