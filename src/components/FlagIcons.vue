<template>
  <span class="fi" :class="`fi-${countryCode}`"></span>
</template>

<script>
export default {
  name: "FlagIcons",
  props: {
    languageCode: String,
  },
  computed: {
    countryCode: function () {
      switch (this.languageCode) {
        case "ru":
          return "ru";
        case "de":
          return "de";
        case "fr":
          return "fr";
        case "it":
          return "it";
        case "en":
          return "gb";
        case "es":
          return "es";
        case "pl":
          return "pl";
        case "uk":
          return "ua";
        case "ro":
          return "ro";
        case "nl":
          return "nl";
        case "tr":
          return "tr";
        case "el":
          return "gr";
        case "hu":
          return "hu";
        case "sv":
          return "se";
        case "cs":
          return "cz";
        case "pt":
          return "pt";
        case "sr":
          return "rs";
        case "bg":
          return "bg";
        case "hr":
          return "hr";
        case "da":
          return "dk";
        case "fi":
          return "fi";
        case "sq":
          return "al";
        case "no":
          return "no";
        case "sk":
          return "sk";
        case "zh":
          return "cn";
        case "hi":
          return "in";
        case "bn":
          return "bd";
        case "ja":
          return "jp";
        case "ko":
          return "kr";
        case "pa":
          return "in";
        case "ur":
          return "pk";
        case "ta":
          return "in";
        case "vi":
          return "vn";
        case "mr":
          return "in";
        case "te":
          return "in";
        case "jv":
          return "id";
        case "ar":
          return "ara";
        case "fa":
          return "ir";
        case "ml":
          return "in";
        case "kn":
          return "in";
        default:
          return "unknown";
      }
    },
  },
};
</script>

<style lang="scss" scoped>
@import "~flag-icons/css/flag-icons.css";
.fi-ara {
  background-image: url(../assets/ara.svg);
}
.fi-unknown {
  background-image: url(../assets/unknown.svg);
}
</style>
