<template>
  <div class="articles">
      <div class="row justify-content-between container-xl m-auto">
        <div class="col-lg-2 col-sm-12">
          <h3 class="articles__title">Статьи</h3>
        </div>
      </div>
    <div class="articles__wrapp  ">
    

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
        v-if='article.theme==theme.theme && article.hot==true'
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
    <div class="container-lg">
      <div class="link-to-all">
      <router-link to="/Articles" class='w-100 d-flex align-items-center justify-content-between'>
         Все статьи
      <svg width="44" height="20" viewBox="0 0 44 20" fill="none" xmlns="http://www.w3.org/2000/svg">
      <path d="M1 10H43M43 10L35.6111 1M43 10L35.6111 19" stroke="white" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"/>
      </svg>
      </router-link>
    </div>
    </div>

    </div>


  </div>
</template>

<script>

// import Articles from '@/components/Articles'
import ArticleCard from '@/components/subcomponents/Article-Card';
import { Swiper, SwiperSlide, directive } from 'vue-awesome-swiper'
import 'swiper/swiper-bundle.css'
import {mapState} from 'vuex'
import $ from 'jquery';

export default {
  name: 'Articles1',
  components: {
    // Articles
    ArticleCard,
    Swiper,
    SwiperSlide
  },
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
  this.swiper.slideTo(0, 1000, false)
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
};
</script>

<style lang="scss" scope>

@import '../scss/variables';

.articles {
  background-image: url('~@/assets/dots-articles.svg');
  background-repeat: no-repeat;
  background-color: #f2f4f9;
  padding-bottom: 130px;
  overflow: hidden;
  ul{
    li{
      list-style-type:none ;
      cursor: pointer;
      p{
        margin-bottom: 0;
      }
    }
  }
}

.link-to-all{
  background-color: $base-green;
  color: #fff;
  padding: 16px 24px;
  width: 226px;
  float: right;
  border-radius: 10px;
  a{
    color: #fff;
  }
}


.articles__title {
  font-family: 'San Francisco' sans-serif;
  font-size: 80px;
  font-style: normal;
  font-weight: 700;
  line-height: 88px;
  letter-spacing: 2px;
  text-align: left;
}
.articles__items {
  font-size: 16px;
  line-height: 32px;
  letter-spacing: 1px;
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
.article__card--wrap {
  position: relative;
  height: 420px;
}

.articles__card {
  position: relative;
  top: 0px;
  left: 10px;
  z-index: 5;
  height: auto;
  width: 642px;
  left: 325px;
  top: 3088px;
  border-radius: 15px;
  background-color: #fff;
  box-shadow: 20px 24px 32px 0px rgba(108, 137, 164, 0.15);
  &::before {
    content: '';
    position: relative;
    top: 70px;
    right: 220px;
    height: 8px;
    background-image: url('~@/assets/line.svg');
    background-repeat: no-repeat;
  }
}
.card__subject {
  height: 28px;
  width: fit-content;
  padding-left: 5px;
  padding-right: 5px;
  margin-top: 32px;
  padding-top: 3px;
  text-align: center;
  box-sizing: border-box;
  border-radius: 10px;
  color: #fff;
  background: black;
}

.card__date {
  margin-top: 10px;
  margin-bottom: 10px;
  color: #828282;
}
.card__text {
  width: 480px;
  font-style: normal;
  font-weight: 600;
  font-size: 32px;
  line-height: 38px;
  letter-spacing: 0.02em;

  color: #000000;
}
.articles__img {
  position: relative;
  top: -200px;
  left: -16px;
  z-index: 3;
  height: 271px;
  width: 584px;
  border-radius: 15px;
}

@media (max-width:576px) {
  .articles__title{
  font-size: 32px;
  line-height: 120%;
  }

  .active-button{
    padding: 24px 8px;
  }
  
  .articles__items{
    margin-top: 40px;
    margin-bottom: 60px;  
    }
    .article__card--wrap{
      display: none;
    }
    .card__wrapp{
      background-color:  transparent;
      box-shadow: none;
    }

  .articles__wrapp{
    ul{
      display: flex;
      li{
        width: 25%;
      }
    }
  }
}
</style>
