<template>
  <div class="blog-inner">
    <div class="display-error" v-if="!isEmpty(blogItem)">
      <div class="container">
        <h2>Page not found</h2>
        <nuxt-link class="is-size-3" :to="'/blog'">Go to Blog</nuxt-link>
      </div>
    </div>
    <div class="container" v-if="isEmpty(blogItem)">
      <div class="blog-container">
        <div class="top-image" :style="{ 'background-image': `url(${blogItem.image.data})` }"></div>
        <div class="blog-content">
          <div class="b-sub">{{getCategory(blogItem.category_id)}}</div>
          <div class="b-title">{{blogItem.title}}</div>
          <div class="is-flex">
            <div class="b-date">{{ blogItem.created_at | moment('MMMM DD, YYYY') }}</div>
            <div class="divider">|</div>
            <div class="b-author">{{blogItem.author}}</div>
          </div>

          <div class="b-content" v-html="blogItem.content"></div>
        </div>
      </div>
      <div class="read-more">Read More</div>
      <div class="blog-jumper">
        <div class="columns is-multiline">
          <div class="column is-2" v-for="blog in blogList" :key="blog.id">
            <nuxt-link :to="`/blog/${blog.slug}`">
              <div class="item-container">
                <div class="b-img" :style="{ 'background-image': `url(${blog.image.data})` }"></div>
                <div class="b-title">{{blog.title}}</div>
              </div>
            </nuxt-link>
          </div>
        </div>
      </div>
      <section class="section blog-inner">
        <b-pagination
          class="is-rounded b-paginator"
          :total="total"
          @change="setPage($event)"
          :per-page="perPage"
          :current.sync="current"
        />
      </section>
    </div>
  </div>
</template>

<script>
import PageMixin from "@/mixins/blog.id";
export default {
  mixins: [PageMixin],
  layout: "blog",
  head() {
    return {
      title: `${this.blogItem.title} | Prudent Domiciliary Care`,
      meta: [
        // hid is used as unique identifier. Do not use `vmid` for it as it will not work
        {
          hid: "title",
          name: "title",
          content: `Prudent Domiciliary Blog : ${this.blogItem.title}`
        },
        {
          hid: "description",
          name: "description",
          content: `${this.blogItem.title}. Prudent Domiciliary Care`
        },
        {
          hid: "keywords",
          name: "keywords",
          content: `${this.blogItem.name}, Bexely, Care, BexleyCare, Bexely Dementia Care, Home Care United Kingdom, Elder Care `
        }
      ]
    };
  },
  methods: {
    getCategory(id) {
      let category = this.services.find(c => c.id == id);

      return category.name;
    }
  }
};
</script>

<style lang="scss" scoped>
</style>
