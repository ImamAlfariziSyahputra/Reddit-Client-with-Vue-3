<template>
  <div class="home">
    REDDIT CLIENT
    <div v-if="postsState.loading" class="progress green lighten-3">
        <div class="indeterminate"></div>
    </div>
    <div v-if="postsState.error" class="card red accent-1">
      <div class="card-content white-text">
        <span class="card-title">{{ postsState.error }}</span>
      </div>
    </div>
    <div class="row">
      <div class="col s6" v-for="post in posts" :key="post.id">
        <div class="card post">
          <div v-if="isVideo(post)" class="card-image waves-effect waves-block waves-light">
            <video class="activator video" controls muted autoplay>
              <source type="video/mp4" :src="getVideoUrl(post)"/>
            </video>
          </div>
          <div v-if="isImage(post.url)" class="card-image waves-effect waves-block waves-light">
            <img class="activator" :src="post.url">
          </div>
          <div class="card-content">
            <span class="card-title activator grey-text text-darken-4">{{ post.title }}</span>
            <p><a :href="`https://www.reddit.com${post.permalink}`">Comments</a></p>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import usePosts from '@/hooks/usePosts';
import { computed } from 'vue';

export default {
  setup() {
    const postsState = usePosts('aww');

    const posts = computed(() => postsState.data.map((child) => child.data));

    function isVideo(post) {
      return post.media || post.url.match(/mp4|gifv|mkv|webm/);
    }

    function isImage(url) {
      return url.match(/bmp|webp|png|jpg|jpeg|gif/);
    }

    function getVideoUrl(post) {
      // if this post already has mp4 ext
      if (post.media) {
        return post.media.reddit_video.scrubber_media_url;
      }
      // if this post is not mp4 ext, change into mp4
      // example : .gifv
      const parts = post.url.split('.'); // split the dot
      parts.pop(); // delete .gifv ext
      return parts.concat('mp4').join('.'); // concat with dot
    }

    return {
      postsState,
      posts,
      isImage,
      isVideo,
      getVideoUrl,
    };
  },
};
</script>
<style>
.post {
  height: 100%;
}
.video {
  width: 100%;
}
</style>
