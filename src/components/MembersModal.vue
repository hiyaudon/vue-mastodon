<template>
  <transition name="modal">
    <div class="modal--mask">
      <div class="modal--wrapper">
        <div class="modal--container">
          <button class="btn modal-default-button" @click="$emit('close')">
            <i class="fas fa-times fa-lg"></i>
          </button>

          <div class="modal--body">
            <carousel
              ref="carousel"
              :per-page="1" 
              :navigation-enabled="true" 
              :pagination-enabled="false"
              :navigate-to="[defaultPage, false]"
              navigation-prev-label='<i class="fas fa-angle-left fa-2x"></i>'
              navigation-next-label='<i class="fas fa-angle-right fa-2x"></i>'
            >
              <slide v-for="user in users" :key="user.username">
                <div class="row box-inner">
                  <div class="col-md-6 mb-4 position-relative">
                    <div class="position-absolute">
                      <img class="thumbnail rounded-circle" :src="user.avatar_static" :alt="$parent.name(user)"> 
                    </div>
                    <img class="img-fluid user-image" src="@/assets/user-images/sample.png" alt="">
                  </div>

                  <div class="col-md-6 l-col-info">
                    <aside class="heading mb-4">
                      <img src="@/assets/heading-top.png" class="img-fluid" />
                      <div class="py-2">
                        <h3 class="mb-2" v-html="$parent.nameHtml(user)"></h3>
                        <div>@{{user.username}}</div>
                      </div>
                      <img src="@/assets/heading-bottom.png" class="img-fluid" />
                    </aside>

                    <aside class="mb-4">
                      <h4 class="mb-2"><i class="fas fa-utensils mr-4"></i>好きなお肉</h4>
                      <div>{{comment(user).niku}}</div>
                    </aside>

                    <aside class="l-comments">
                      <h4 class="mb-2"><i class="fas fa-comment mr-4"></i>フェスの意気込み</h4>
                      <div class="comment">{{comment(user).comment}}</div>
                    </aside>

                  </div>
                </div>
              </slide>
            </carousel>
          </div>

        </div>
      </div>
    </div>
  </transition>

</template>

<script>
import { Carousel, Slide } from 'vue-carousel';

const COMMENTS = require('@/assets/comments.json')

export default {
  name: 'MembersModal',
  components: {
    Carousel,
    Slide
  },
  props: {
    defaultPage: { // 初期表示時のページ番号
      type: Number,
      default: 1,
    },
    users: Array
  },
  methods: {
    comment(user) {
      return COMMENTS.filter(data => data.username == user.username).shift()
    }
  },
}
</script>

<style lang="scss">
$modalBackgroundColor: #FF8383;

.modal--mask {
  position: fixed;
  z-index: 9998;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(255,203,203, .9);
  display: table;
  transition: opacity .3s ease;
}

.modal--wrapper {
  display: table-cell;
  vertical-align: middle;
}

.modal--container {
  width: 90vw;
  margin: 0px auto;
  padding: 20px 30px;
  background-color: $modalBackgroundColor;
  border-radius: .5rem;
  box-shadow: 0 2px 8px rgba(0, 0, 0, .33);
  transition: all .3s ease;
  font-family: Helvetica, Arial, sans-serif;
  position: relative;
  max-height: 95%;
}

.modal-default-button {
  position: absolute;
  top: 0;
  right: 0;
}

/*
 * The following styles are auto-applied to elements with
 * transition="modal" when their visibility is toggled
 * by Vue.js.
 *
 * You can easily play with the modal transition by editing
 * these styles.
 */

.modal-enter {
  opacity: 0;
}

.modal-leave-active {
  opacity: 0;
}

.modal-enter .modal-container,
.modal-leave-active .modal-container {
  -webkit-transform: scale(1.1);
  transform: scale(1.1);
}

img.user-image {
  width: 100%;
}
img.thumbnail {
  width: 80px;
  top: .5rem;
  left: 1.5rem;
  z-index: 1;

  $imgBorderWidth: 5px;
  border: $imgBorderWidth $modalBackgroundColor solid;
  background-color: $modalBackgroundColor; // 透過pngの画像とかあるので、bgcolor入れときます
  height: 80px + $imgBorderWidth;
  width: 80px + $imgBorderWidth;
}
.l-col-info {
  h3,
  h4 {
    font-weight: bold;
  }
  .emoji {
    margin: -.6ex 0 .2ex;
    width: 30px;
    height: 30px;  
  }
}
.l-comments {
  max-height: 10vw;
  white-space: pre-wrap;
  word-wrap: break-word;
}

.VueCarousel-navigation-button {
  color: #4C3535 !important;
  &:focus {
    outline: 1px solid transparent !important;
  }
}
.row.box-inner {
  overflow: hidden;
  overflow-y: auto;
}
// .VueCarousel-wrapper {
//   .VueCarousel-slide {
//     overflow: hidden;
//     overflow-y: auto;
//   }
// }
</style>
