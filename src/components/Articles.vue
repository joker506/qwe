<template>
<div class='articles'>  
      <ul
      class="articles__items switcher col-lg-4 offset-lg-2 d-lg-flex d-none justify-content-between"
      >
      <li 
      v-for='(theme, index) in themes' 
      :key="index" >
      <p
        class='articles__item w-100 h-100 '
        :class="{'active-button': index==0}">
        {{theme.theme}}
      </p>

    </li>
    </ul>
    <swiper class='switcher articles__items slider d-lg-none d-block' ref="mySwiper" :options="swiperOptions" >
      <swiper-slide 
      v-for='(theme, index) in themes' 
      :key="index" >
        <p :class="{'active-button': index==0}" class='articles__item w-100 d-flex h-100 justify-content-center align-items-center'>{{theme.theme}}</p>
      </swiper-slide>
    </swiper>
    <div
    v-for='(theme, index) in themes' :key=theme.theme
    class='themes'
    >
      <div v-for='(article) in articles' :key='article.title'
      class=themesTwo
      >
        <div
        v-if='article.theme==theme.theme'
        :class="{'d-none': index != 0}"
        class='art-cat'
        >
          <ArticleCard
          :title = article.title
          :content = article.content
          :date = article.date
          :author = article.author
          :img = article.img
          />
        </div>
      </div>
    </div>

</div>
</template>


<script>
import ArticleCard from '@/components/subcomponents/Article-Card';
import $ from 'jquery';
import { Swiper, SwiperSlide, directive } from 'vue-awesome-swiper'
import 'swiper/swiper-bundle.css'
import {mapState} from 'vuex'
export default {
name: 'ArticlesComponent',

  computed:{
  ...mapState({
        articles: state => state.articlesInfo.Articles,
        themes: state => state.articlesInfo.Themes
    }),
    swiper() {
      return this.$refs.mySwiper.$swiper
    }
  },
directives: {
    swiper: directive
  },
components: {
  ArticleCard,
  Swiper,
  SwiperSlide
},
data() {
    return{
      swiperOptions: {
        slidesPerView: 4,
        spaceBetween: 15
      }
    }
  },
mounted(){
  let len = this.themes.length;
  // switcher
  $('.switcher li').each(function(i){
    $(this).click(function(){
      $(this).children('p').addClass('active-button')
      $(this).siblings().children('p').removeClass('active-button')
      $(this).parent().parent().children('.themes').eq(i).children('.themesTwo').children('.art-cat').removeClass('d-none')
      for (let j=1; j<len;j++){
        $(this).parent().parent().children('.themes').eq(i+j).children('.themesTwo').children('.art-cat').addClass('d-none')
        $(this).parent().parent().children('.themes').eq(i-j).children('.themesTwo').children('.art-cat').addClass('d-none')
      }
    })
  })
  $('.swiper-slide').each(function(i){
    $(this).click(function(){
      $(this).children('p').addClass('active-button')
      $(this).siblings().children('p').removeClass('active-button')
      $(this).parent().parent().parent().children('.themes').eq(i).children('.themesTwo').children('.art-cat').removeClass('d-none')
      for (let j=1; j<len;j++){
        $(this).parent().parent().parent().children('.themes').eq(i+j).children('.themesTwo').children('.art-cat').addClass('d-none')
        $(this).parent().parent().parent().children('.themes').eq(i-j).children('.themesTwo').children('.art-cat').addClass('d-none')
      }
    })
  })
}
}
</script>

<style lang='scss'>
@import '../scss/variables';
$distance: 250px;



.articles__items {
  font-size: 16px;
  line-height: 32px;
  letter-spacing: 1px;
}

.green {
  width: $distance;
  height: 8px;
  content: '';
  z-index: 1;
  background-color: $base-green;
}


.active-button{
  color: #fff;
  background-color: rgba(102, 209, 131, 1);
}

.articles__item {
  text-transform: uppercase;
  text-align: center;
  list-style: none;
  box-sizing: border-box;
  padding: 6px 4px;
  height: 48px;
  width: 112px;

  &:hover {
    cursor: pointer;
    color: #fff;
    background-color: rgba(102, 209, 131, 1);
  }
}
</style>