<template>
  <div :class="menuOpen ? 'open' : ''" class="mobile-language-selector">
    <div class="backdrop" @click="$emit('isopen', false)"></div>
    <div class="language-menu-content-container">
      <ul>
        <li
          v-for="language in supportedLanguages"
          :key="language.key"
          :active="$root.$i18n.locale === language.langCode"
          :data-language-code="language.langCode"
          :data-flag-name="language.flag"
          @click="languageItemClicked"
        >
          {{ language.name }}
        </li>
      </ul>
    </div>
  </div>
</template>

<script>
import { mapActions } from 'vuex';
import supportedLang from '@/containers/HeaderContainer/supportedLang';

export default {
  props: {
    open: {
      type: Boolean,
      default: false
    }
  },
  data() {
    return {
      currentName: 'English',
      currentFlag: 'en',
      menuOpen: false,
      supportedLanguages: supportedLang
    };
  },
  watch: {
    open(newVal) {
      this.menuOpen = newVal;
    }
  },
  methods: {
    ...mapActions('main', ['setLocale']),
    languageItemClicked(e) {
      const code = e.target.getAttribute('data-language-code') || 'en_US';
      const flag = e.target.getAttribute('data-flag-name') || 'en';

      this._i18n.locale = code;
      this.currentName = e.target.innerText.replace(/^\s+|\s+$|\s+(?=\s)/g, '');
      this.currentFlag = flag;

      this.setLocale({ locale: code, save: true });
      this.$emit('isopen', false);
      this.$emit('currentlang', this.currentName);
      this.$emit('currentflag', this.currentFlag);
    }
  }
};
</script>

<style lang="scss" scoped>
@import 'MobileLanguageSelector.scss';
</style>
