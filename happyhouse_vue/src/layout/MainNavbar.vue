<template>
  <md-toolbar
    id="toolbar"
    md-elevation="0"
    class="md-transparent md-absolute"
    :class="extraNavClasses"
    :color-on-scroll="colorOnScroll"
  >
    <div class="md-toolbar-row md-collapse-lateral">
      <div class="md-toolbar-section-start">
        <router-link :to="{ name: 'home' }">
          <h3 class="md-title" style="font-size: 26px">HAPPY HOUSE</h3>
        </router-link>
      </div>
      <div class="md-toolbar-section-end">
        <div class="md-collapse">
          <div class="md-collapse-wrapper">
            <mobile-menu nav-mobile-section-start="false">
              <!-- Here you can add your items from the section-start of your toolbar -->
            </mobile-menu>
            <md-list>
              <li class="md-list-item">
                <a
                  href="javascript:void(0)"
                  class="md-list-item-router md-list-item-container md-button-clean dropdown"
                >
                  <div class="md-list-item-content">
                    <drop-down direction="down">
                      <md-button
                        slot="title"
                        class="md-button md-simple md-white"
                        data-toggle="dropdown"
                      >
                        <i class="material-icons">house</i>
                        <p style="font-size: 13pt">APT INFO ▼</p>
                      </md-button>
                      <ul class="dropdown-menu dropdown-with-icons">
                        <li>
                          <router-link :to="{ name: 'house' }">
                            <i class="material-icons">apartment</i>
                            <p>아파트 조회</p>
                          </router-link>
                        </li>
                        <li>
                          <router-link :to="{ name: 'around' }">
                            <i class="material-icons">rooms</i>
                            <p>주변 둘러보기</p>
                          </router-link>
                        </li>
                        <li>
                          <router-link :to="{ name: 'interest' }">
                            <i class="material-icons">favorite</i>
                            <p>관심 매물</p>
                          </router-link>
                        </li>
                      </ul>
                    </drop-down>
                  </div>
                </a>
              </li>

              <li class="md-list-item">
                <a
                  href="javascript:void(0)"
                  class="md-list-item-router md-list-item-container md-button-clean dropdown"
                >
                  <div class="md-list-item-content">
                    <drop-down direction="down">
                      <md-button
                        slot="title"
                        class="md-button md-simple md-white"
                        data-toggle="dropdown"
                      >
                        <i class="material-icons">assignment </i>
                        <p style="font-size: 13pt">BOARD ▼</p>
                      </md-button>
                      <ul class="dropdown-menu dropdown-with-icons">
                        <li>
                          <router-link :to="{ name: 'notice' }">
                            <i class="material-icons">campaign</i>
                            <p>공지사항</p>
                          </router-link>
                        </li>
                        <li>
                          <router-link :to="{ name: 'comm' }">
                            <i class="material-icons">question_answer</i>
                            <p>커뮤니티</p>
                          </router-link>
                        </li>
                        <li>
                          <router-link :to="{ name: 'qna' }">
                            <i class="material-icons">helps</i>
                            <p>Q&A</p>
                          </router-link>
                        </li>
                      </ul>
                    </drop-down>
                  </div>
                </a>
              </li>
              <li class="md-list-item">
                <a
                  href="javascript:void(0)"
                  class="md-list-item-router md-list-item-container md-button-clean dropdown"
                >
                  <div class="md-list-item-content">
                    <drop-down direction="down">
                      <md-button
                        slot="title"
                        class="md-button md-button-link md-white md-simple"
                        data-toggle="dropdown"
                      >
                        <i class="material-icons">account_circle</i>
                        <p v-if="userInfo" style="font-size: 13pt">
                          {{ userInfo.username }}({{ userInfo.userid }})님
                          환영합니다. ▼
                        </p>
                        <p v-else style="font-size: 13pt">
                          로그인 후 이용해주세요. ▼
                        </p>
                      </md-button>
                      <ul class="dropdown-menu dropdown-with-icons">
                        <li>
                          <router-link
                            :to="{ name: 'register' }"
                            v-if="!userInfo"
                          >
                            <i class="material-icons">account_box</i>
                            <p>회원가입</p>
                          </router-link>
                        </li>
                        <li>
                          <router-link :to="{ name: 'login' }" v-if="!userInfo">
                            <i class="material-icons">fingerprint</i>
                            <p>로그인</p>
                          </router-link>
                        </li>
                        <li>
                          <a
                            href=""
                            @click.prevent="onClickLogout"
                            v-if="userInfo"
                          >
                            <i class="material-icons">logout</i>
                            <p>로그아웃</p>
                          </a>
                        </li>
                        <li>
                          <router-link :to="{ name: 'mypage' }" v-if="userInfo">
                            <i class="material-icons">perm_identity</i>
                            <p>마이페이지</p>
                          </router-link>
                        </li>
                      </ul>
                    </drop-down>
                  </div>
                </a>
              </li>
            </md-list>
          </div>
        </div>
      </div>
    </div>
  </md-toolbar>
</template>

<script>
let resizeTimeout;
function resizeThrottler(actualResizeHandler) {
  // ignore resize events as long as an actualResizeHandler execution is in the queue
  if (!resizeTimeout) {
    resizeTimeout = setTimeout(() => {
      resizeTimeout = null;
      actualResizeHandler();

      // The actualResizeHandler will execute at a rate of 15fps
    }, 66);
  }
}

import MobileMenu from "@/layout/MobileMenu";
import { mapState, mapMutations } from "vuex";

export default {
  components: {
    MobileMenu,
  },
  props: {
    type: {
      type: String,
      default: "white",
      validator(value) {
        return [
          "white",
          "default",
          "primary",
          "danger",
          "success",
          "warning",
          "info",
        ].includes(value);
      },
    },
    colorOnScroll: {
      type: Number,
      default: 0,
    },
  },
  data() {
    return {
      extraNavClasses: "",
    };
  },
  computed: {
    ...mapState(["isLogin", "userInfo"]),
  },
  methods: {
    ...mapMutations(["SET_IS_LOGIN", "SET_USER_INFO"]),

    onClickLogout() {
      // console.log("memberStore : ", ms);
      alert("로그아웃 되었습니다.");
      this.SET_IS_LOGIN(false);
      this.SET_USER_INFO(null);
      sessionStorage.removeItem("access-token");
      if (this.$route.path != "/") this.$router.push({ name: "home" });
    },

    bodyClick() {
      let bodyClick = document.getElementById("bodyClick");

      if (bodyClick === null) {
        let body = document.querySelector("body");
        let elem = document.createElement("div");
        elem.setAttribute("id", "bodyClick");
        body.appendChild(elem);

        let bodyClick = document.getElementById("bodyClick");
        bodyClick.addEventListener("click", this.toggleNavbarMobile);
      } else {
        bodyClick.remove();
      }
    },
    toggleNavbarMobile() {
      this.NavbarStore.showNavbar = !this.NavbarStore.showNavbar;
      this.toggledClass = !this.toggledClass;
      this.bodyClick();
    },
    handleScroll() {
      let scrollValue =
        document.body.scrollTop || document.documentElement.scrollTop;
      let navbarColor = document.getElementById("toolbar");
      this.currentScrollValue = scrollValue;
      if (this.colorOnScroll > 0 && scrollValue > this.colorOnScroll) {
        this.extraNavClasses = `md-${this.type}`;
        navbarColor.classList.remove("md-transparent");
      } else {
        if (this.extraNavClasses) {
          this.extraNavClasses = "";
          navbarColor.classList.add("md-transparent");
        }
      }
    },
    scrollListener() {
      resizeThrottler(this.handleScroll);
    },
  },
  mounted() {
    document.addEventListener("scroll", this.scrollListener);
  },
  beforeDestroy() {
    document.removeEventListener("scroll", this.scrollListener);
  },
};
</script>
