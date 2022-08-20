<template>
  <div class="contact">
    <g-image
      class="contact__icon"
      :src="require(`!!assets-loader!@images/${iconURL}`)"
      width="30"
    />
    <span v-if="type === 'text'" ref="contactName" class="contact__value">
      {{ value }}
    </span>
    <Link v-else class="contact__value" :to="value">{{ value }}</Link>
    <g-image
      v-if="type === 'text'"
      class="contact__clipboard"
      role="button"
      src="~/assets/clipboard.svg"
      width="30"
      @click="copy"
    />
    <span v-show="copied" class="contact__copied">Copied!</span>
  </div>
</template>

<script>
import Link from "./Link.vue";
export default {
  name: "ContactItem",
  components: {
    Link,
  },
  props: {
    iconURL: String,
    value: String,
    type: {
      type: String,
      validator(value) {
        return ["link", "text"].includes(value);
      },
    },
  },
  data() {
    return {
      copied: false,
    };
  },
  methods: {
    onCopied() {
      this.copied = true;
      setTimeout(() => {
        this.copied = false;
      }, 1500);
    },
    copy() {
      const range = document.createRange();
      const selection = window.getSelection();
      range.selectNodeContents(this.$refs.contactName);

      selection.removeAllRanges();
      selection.addRange(range);

      document.execCommand("copy");
      selection.removeAllRanges();

      this.onCopied();
    },
  },
};
</script>

<style scoped>
.contact {
  position: relative;
  padding: 8px 16px;
  display: flex;
  justify-content: space-evenly;
  align-items: center;
  background-color: var(--secondary);
  color: var(--light);
  border-radius: 10px;
}

.contact__icon {
  padding: 2px;
  width: 30px;
  margin-right: 10px;
  border-radius: 50%;
  background-color: var(--light);
}

.contact__value {
  flex-grow: 1;
}

.contact__clipboard {
  margin-left: 40px;
  cursor: pointer;
}

.contact__copied {
  padding: 8px 16px;
  background-color: green;
  position: absolute;
  left: 70%;
  right: -10%;
  top: 0;
  bottom: 0;
  border-radius: 10px;
  text-align: center;
}
</style>
