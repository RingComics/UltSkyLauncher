<template>
  <div class="l-column u-scroll-y" v-if="!failedToGetNews && news.length > 0">
    <ul class="c-news u-list--bare">
      <li v-for="newsItem in news" :key="newsItem.key" class="c-news__item">
        <ExternalLink
          :href="`https://www.patreon.com${newsItem.url}`"
          class="l-row"
        >
          <div class="c-news__published">
            {{ new Date(newsItem.published).toLocaleDateString() }}
          </div>
          <div class="c-news__block l-column">
            <div class="c-news__tags l-row">
              <template v-if="newsItem.tags">
                <div
                  v-for="tag in newsItem.tags.slice(0, 4)"
                  :key="tag.key"
                  :class="[
                    tag.toUpperCase() === 'NEWS' && 'c-news__tag--news',
                    tag.toUpperCase() === 'DEVSTREAM' &&
                      'c-news__tag--devstream'
                  ]"
                  class="c-news__tag"
                >
                  {{ tag.toUpperCase() }}
                </div>
              </template>
              <template v-else>
                <div class="c-news__tag c-news__tag--news">
                  News
                </div>
              </template>
            </div>
            <div class="c-news__content">
              {{ newsItem.title }}
            </div>
          </div>
        </ExternalLink>
      </li>
    </ul>
  </div>
  <div v-if="failedToGetNews && news.length === 0">
    Cannot load latest news. Please report this error in the
    <ExternalLink href="https://discordapp.com/invite/8VkDrfq" :underline="true"
      >Ultimate Skyrim Discord</ExternalLink
    >.
  </div>
</template>

<script lang="ts">
import { Options as Component, Vue } from "vue-class-component";
import List from "./List.vue";
import { Posts, PostsService } from "@/services/posts.service";
import { injectStrict, SERVICE_BINDINGS } from "@/services/service-container";
import ExternalLink from "@/components/ExternalLink.vue";

@Component({
  components: {
    List,
    ExternalLink
  }
})
export default class News extends Vue {
  newsService!: PostsService;
  news: Posts[] = [];
  failedToGetNews = false;

  async created() {
    this.newsService = injectStrict(SERVICE_BINDINGS.NEWS_SERVICE);
    try {
      this.news = await this.newsService.getPosts();
    } catch {
      this.failedToGetNews = true;
    }
  }
}
</script>

<style lang="scss" scoped>
@import "~@/assets/scss";

.c-news {
  margin: $size-margin--large;
}

.c-news__item {
  margin-bottom: $size-margin;
}

.c-news__published {
  color: $colour-text-secondary;
  flex: 0.2;
}

.c-news__content {
  font-size: $font-size--small;
  margin-top: $size-margin--x-small;
}

.c-news__block {
  flex: 0.8;
}

.c-news__tag {
  font-size: $font-size--x-small;
  font-weight: $font-weight--large;

  padding: 3px 6px;
  margin-right: $size-margin--x-small;

  border-radius: 8px;
  background-color: $colour-background--dark;
}

.c-news__tag--news {
  background-color: $colour-primary;
}

.c-news__tag--devstream {
  background-color: $colour-alternate;
}
</style>
