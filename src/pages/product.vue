<template>
  <div class="product">
    <product-param :title="product.name">
      <template v-slot:buy>
        <button class="btn" @click="goDetail">立即购买</button>
      </template>
    </product-param>
    <div class="content">
      <div class="item-bg">
        <h2>{{ product.name }}</h2>
        <h3>{{ product.subImages }}</h3>
        <p>
          <a href="" id="">全球首款双频 GP</a>
          <span>|</span>
          <a href="" id="">骁龙845</a>
          <span>|</span>
          <a href="" id="">AI 变焦双摄</a>
          <span>|</span>
          <a href="" id="">红外人脸识别</a>
        </p>
        <div class="price">
          <span
            >￥<em>{{ product.price }}</em></span
          >
        </div>
      </div>
      <div class="item-bg-2"></div>
      <div class="item-bg-3"></div>
      <div class="item-swiper">
        <swiper :options="swiperOption">
          <swiper-slide
            ><img src="/imgs/product/gallery-2.png" alt=""
          /></swiper-slide>
          <swiper-slide
            ><img src="/imgs/product/gallery-3.png" alt=""
          /></swiper-slide>
          <swiper-slide
            ><img src="/imgs/product/gallery-4.png" alt=""
          /></swiper-slide>
          <swiper-slide
            ><img src="/imgs/product/gallery-5.jpg" alt=""
          /></swiper-slide>
          <swiper-slide
            ><img src="/imgs/product/gallery-6.jpg" alt=""
          /></swiper-slide>
          <!-- Optional controls -->
          <div class="swiper-pagination" slot="pagination"></div>
        </swiper>
        <p class="disc">小米8 AI变焦双摄拍摄</p>
      </div>
      <div class="item-video">
        <h2>60帧超慢动作摄影<br />慢慢回味每一瞬间的精彩</h2>
        <p>
          后置960帧电影般超慢动作视频，将眨眼间的美妙展现得淋漓尽致！<br />更能AI
          精准分析视频内容，15个场景智能匹配背景音效。
        </p>
        <div class="video-bg" @click="showSlide = 'slideDowm'"></div>
        <div class="video-box" v-show="showSlide">
          <!-- 遮罩层 -->
          <div class="overlay"></div>
          <div class="video" :class="showSlide">
            <video
              src="/imgs/product/video.mp4"
              controls="controls"
              muted
              autoplay
            ></video>
            <span class="icon-close" @click="closeVideo"></span>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { swiper, swiperSlide } from "vue-awesome-swiper";
import "swiper/dist/css/swiper.css";
import ProductParam from "../components/ProductParam.vue";
export default {
  name: "product",
  data() {
    return {
      showSlide: "",
      swiperOption: {
        autoplay: true,
        slidesPerView: 3,
        spaceBetween: 30,
        freeMode: true,
        pagination: {
          el: ".swiper-pagination",
          clickable: true,
        },
      },
      product: {},
    };
  },
  components: {
    ProductParam,
    swiper,
    swiperSlide,
  },
  mounted() {
    this.getProduct();
  },
  methods: {
    closeVideo() {
      this.showSlide = "slideUp";
      setTimeout(() => {
        this.showSlide = "";
      }, 600);
    },
    getProduct() {
      let id = this.$route.params.id;
      this.axios.get(`/products/${id}`).then((res) => {
        this.product = res;
      });
    },
    goDetail(){
      let id = this.$route.params.id;
      this.$router.push({path:`/detail/${id}`})
    },
  },
};
</script>

<style lang="scss">
@import "./../assets/scss/mixin.scss";
.product {
  .btn {
    margin-left: 10px;
  }
  .content {
    .item-bg {
      background: url("/imgs/product/product-bg-1.png") no-repeat center;
      height: 718px;
      text-align: center;
      h2 {
        font-size: 80px;
        padding-top: 55px;
      }
      h3 {
        font-size: 24px;
        letter-spacing: 10px;
      }
      p {
        margin-top: 21px;
        margin-bottom: 40px;
      }
      a {
        font-size: 16px;
        color: #333333;
      }
      span {
        margin: 0 15px;
      }
    }
    .price {
      font-size: 30px;
      color: #333333;
      em {
        font-style: normal;
        font-size: 38px;
      }
    }
    .item-bg-2 {
      background: url(/imgs/product/product-bg-2.png) no-repeat center;
      height: 480px;
      background-size: 1226px auto;
    }
    .item-bg-3 {
      background: url(/imgs/product/product-bg-3.png) no-repeat center;
      height: 638px;
      background-size: cover;
    }
    .item-swiper {
      margin: 36px auto 52px;
      img {
        width: 100%;
      }
      .disc {
        font-size: 18px;
        color: #333333;
        text-align: center;
      }
    }
    .item-video {
      height: 1044px;
      background-color: #070708;
      margin-bottom: 76px;
      color: #ffffff;
      text-align: center;
      h2 {
        font-size: 60px;
        padding-top: 82px;
        margin-bottom: 47px;
      }
      p {
        font-size: 24px;
        margin-bottom: 58px;
      }
      .video-bg {
        width: 1226px;
        height: 540px;
        margin: 0 auto 120px;
        cursor: pointer;
        background: url("/imgs/product/gallery-1.png") no-repeat center;
        background-size: cover;
      }
      .video-box {
        .overlay {
          @include position(fixed);
          background-color: #333333;
          opacity: 0.4;
          z-index: 10;
        }
        @keyframes slideDowm {
          from {
            top: -50%;
          }
          to {
            top: 50%;
          }
        }
        @keyframes slideUp {
          from {
            top: 50%;
          }
          to {
            top: -50%;
          }
        }
        .video {
          width: 1000px;
          height: 536px;
          position: fixed;
          top: -50%;
          left: 50%;
          transform: translate(-50%, -50%);
          z-index: 10;
          transition: all 0.5s;
          &.slideDowm {
            animation: slideDowm 0.6s linear;
            top: 50%;
          }
          &.slideUp {
            animation: slideDowm 0.6s linear;
            top: -50%;
          }
          video {
            width: 100%;
            height: 100%;
            object-fit: cover;
            outline: none;
          }
          .icon-close {
            position: absolute;
            top: 20px;
            right: 20px;
            @include bgimg(20px, 20px, "/imgs/icon-close.png");
            cursor: pointer;
            z-index: 11;
          }
        }
      }
    }
  }
}
</style>