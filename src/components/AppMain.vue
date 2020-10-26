<template>
  <v-card flat class="my-12">
    <v-row no-gutters>
      <v-col cols="10">
        <v-card v-for="item in items" :id="item.link" :key="item.text" dir="rtl" tile class="pt-6">
          <a :name="item.text" :id="item.link">
            <v-card-text>
              <v-card-subtitle dir="rtl">
                <AppForm :section="item.link"/>
              </v-card-subtitle>
            </v-card-text>
          </a>
        </v-card>
      </v-col>
      <v-col cols="2">
        <v-card flat style="position: fixed;" tile>
          <v-list shaped min-width="220px">
            <v-list-item-group
                color="primary"
            >
              <v-list-item
                  v-for="(item, i) in items"
                  :key="i"
                  color="primary"
                  :to="'#' + item.link"
                  :id="item.link"
              >
                <v-list-item-icon>
                  <v-icon v-text="item.icon"></v-icon>
                </v-list-item-icon>
                <v-list-item-content>
                  <v-list-item-title v-text="item.text"></v-list-item-title>
                </v-list-item-content>
              </v-list-item>
            </v-list-item-group>
          </v-list>
        </v-card>
      </v-col>
    </v-row>
  </v-card>
</template>

<script>
import AppForm from "./Accordion/AppForm";

export default {
  components: { AppForm },
  data: () => ({
    items: [
      { text: 'مشخصات اصلی', icon: 'mdi-account', link: 'main' },
      { text: 'مشخصات تکمیلی', icon: 'mdi-check', link: 'complement' },
      { text: 'اطلاعات تماس ها', icon: 'mdi-phone', link: 'communicate' },
      { text: 'اطلاعات آدرس ها', icon: 'mdi-mail', link: 'address' },
    ],
  }),
  created() {
    this.$router.currentRoute.hash !== "#main" ? this.$router.push("#main") :
        window.addEventListener("scroll", function(e) {
          //e.preventDefault();
          const section = e.path[0].anchors;
          section.forEach((el, index) => {
            const offsets = el.getBoundingClientRect();
            //console.log(offsets.top < 50, offsets.height * (index + 1)  > window.scrollY , `#${el.id}` !== this.$router.currentRoute.hash)
            if(offsets.top < 50 && offsets.height * (index + 1)  > window.scrollY) {
              const links = document.querySelectorAll(".v-list-item.v-list-item--link");
              links.forEach(item => {
                console.log(item)
                item.classList.remove("v-list-item--active");
                if(item.innerText == el.name) {
                  item.classList.add("v-list-item--active");
                  //this.$router.push(`#${item.id}`);
                }
              });
            }
          });
        }.bind(this));
    //   changeActiveLink() {
    //  // # If native scroll listener is locked
    //  if (!!this.onScrollHash) {
    //   // # Check if scrolling to lock section is finished or not;
    //   let section = document.querySelector(this.onScrollHash);
    //   if (section.offsetTop !== window.scrollY) {
    //    // # If not, return
    //    return;
    //   } else {
    //    this.onScrollHash = null;
    //   }
    //  }
    //  this.hashedLinks.forEach(link => {
    //   // # Get section to scroll to it
    //   const hash = link.getAttribute("data-section");
    //   if (!!hash) {
    //    let section = document.querySelector(hash);
    //    if (section.offsetTop <= window.scrollY && section.offsetTop + section.offsetHeight > window.scrollY) {
    //     // # Remove all active link, including active class for vuetify
    //     document
    //      .querySelectorAll(".rx-nav__link--active")
    //      .forEach(item => item.classList.remove("rx-nav__link--active", "v-btn--active"));
    //     link.classList.add("rx-nav__link--active");
    //    } else {
    //     link.classList.remove("rx-nav__link--active", "v-btn--active");
    //    }
    //   }
    //  });
  }
}
</script>
