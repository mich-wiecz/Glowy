<template>
  <section class="messenger">
    <div class="messenger__editor editor">
      <div class="editor__title">
        <label class="editor__title-label"> Title </label>
        <input
          class="editor__title-input"
          type="text"
          v-model="messageTitle"
          placeholder="subject of the message"
        />
      </div>
      <ClientOnly>
        <quillEditor class="editor" v-model="message" />
      </ClientOnly>
      <div class="editor__end">
        <div
          v-for="(medium, index) in Object.values(contactMediums)"
          :key="index"
          class="editor__input-group input-group"
          :class="{ enabled: medium.active }"
        >
          <div class="input-group__section">
            <input
              :id="medium.name"
              class="input-group__checkbox"
              type="checkbox"
              v-model="medium.active"
              :disabled="medium.required"
            />
            <label :for="medium.name" class="input-group__label">
              {{ medium.label }}
            </label>
          </div>
          <div class="input-group__section second">
            <input
              :disabled="!medium.active"
              class="input-group__input"
              type="text"
              v-model="medium.value"
            />
            <button
              :disabled="!medium.active"
              class="input-group__btn"
              :class="{ active: preferredMedium === medium.name }"
              @click="changePreferredMedium(medium.name)"
            >
              Preferred
            </button>
          </div>
        </div>
        <button role="submit" class="editor__send-btn" @click="sendMessage">Send</button>
      </div>
    </div>
  </section>
</template>

<script>
import "quill/dist/quill.core.css";
import "quill/dist/quill.snow.css";
import "quill/dist/quill.bubble.css";
import { quillEditor } from "vue-quill-editor";
import emailjs from "emailjs-com";
import { htmlToText } from "html-to-text";
import debounce from "lodash.debounce";
export default {
  name: "MessageSender",
  components: {
    quillEditor,
  },
  data() {
    return {
      messageTitle: "",
      message: "",
      contactMediums: {
        email: {
          name: "email",
          label: "Your email:",
          active: true,
          value: "",
          required: true,
        },
        phone: {
          name: "phone",
          label: "Your phone:",
          active: false,
          value: "",
        },
        otherMedium: {
          name: "otherMedium",
          label: "Other medium:",
          active: false,
          value: "",
        },
      },
      preferredMedium: "email",
      emailId: "36aiopHHzs0qa7Utc",
      emailServiceId: "service_3ountxs",
      emailTemplateId: "template_xzl3edn",
    };
  },
  computed: {
    email() {
      return this.contactMediums.email.value;
    },
    phone() {
      return this.contactMediums.phone.value;
    },
    otherMedium() {
      return this.contactMediums.otherMedium.value;
    },
  },
  methods: {
    changePreferredMedium(medium) {
      if (medium === this.preferredMedium) {
        return;
      }
      this.preferredMedium = medium;
    },
    async sendMessage(e) {
      let loadingToast;
      try {
        loadingToast = this.$toast.info("Sending message..", {
          position: "top",
          duration: 0,
        });

        await emailjs.send(
          this.emailServiceId,
          this.emailTemplateId,
          {
            reply_to: this.contactMediums.email.value,
            phone: this.contactMediums.phone.active
              ? this.contactMediums.phone.value
              : "",
            other_mean: this.contactMediums.otherMedium.active
              ? this.contactMediums.otherMedium.value
              : "",
            email_preferred: this.preferredMedium === "email" ? " - preferred" : "",
            phone_preferred: this.preferredMedium === "phone" ? " - preferred" : "",
            other_preferred: this.preferredMedium === "otherMedium" ? " - preferred" : "",
            subject: this.messageTitle,
            message: htmlToText(this.message, {
              wordwrap: 130,
            }),
          },
          this.emailId
        );

        loadingToast.dismiss();
        this.$toast.success("Message sended!", {
          position: "top",
          duration: 4000,
        });

        this.resetState();
      } catch (error) {
        if (loadingToast) {
          loadingToast.dismiss();
        }
        this.$toast.error("Sending message failed. Try again.", {
          position: "top",
          duration: 6000,
        });
        console.error(error);
      }
    },
    resetState() {
      this.messageTitle = "";
      this.message = "";
    },
    saveToSessionStorage(id, value, isString = true) {
      try {
        sessionStorage.setItem(id, isString ? value : JSON.stringify(value));
      } catch (error) {
        console.error(error);
      }
    },
  },
  watch: {
    messageTitle: debounce(function (title) {
      this.saveToSessionStorage("glowy_message_title", title);
    }, 1000),
    message: debounce(function (message) {
      this.saveToSessionStorage("glowy_message", message);
    }, 1000),
    contactMediums: {
      handler: debounce(function (contacts) {
        this.saveToSessionStorage("glowy_contact_mediums", contacts, false);
      }, 1000),
      deep: true,
    },
    preferredMedium: debounce(function (preferred) {
      this.saveToSessionStorage("glowy_contact_preferred", preferred);
    }, 1000),
  },
  mounted() {
    const title = sessionStorage.getItem("glowy_message_title");
    this.messageTitle = title || "";

    const message = sessionStorage.getItem("glowy_message");
    this.message = message || "";

    const contacts = sessionStorage.getItem("glowy_contact_mediums");
    if (contacts) {
      this.contactMediums = JSON.parse(contacts);
    }

    const preferredMedium = sessionStorage.getItem("glowy_contact_preferred");
    if (preferredMedium) {
      this.preferredMedium = preferredMedium;
    }
  },
};
</script>

<style>
.editor {
  background-color: var(--background);
  color: #ddd;
  box-shadow: 0 0 8px 4px var(--secondary);
}

.ql-container {
  background-color: var(--secondary);
  color: #ddd;
}

.ql-editor {
  min-height: 240px;
  background-color: #ccc;
  color: var(--background);
}

.ql-editor::before {
  color: #444 !important;
}

.ql-toolbar svg,
.ql-toolbar .ql-stroke,
.ql-toolbar * {
  /* fill: white; */
  stroke: var(--light);
  color: #ddd;
}

.ql-toolbar .ql-fill {
  fill: var(--light);
}

.editor__title {
  height: 50px;
  display: flex;
  border: 1px solid #ccc;
  border-bottom: none;
}

.editor__title-label {
  flex-basis: 20%;
  display: flex;
  justify-content: center;
  align-items: center;
  background-color: var(--background);
  color: #ddd;
}

.editor__title-input {
  flex-grow: 1;
  padding: 0 10px;
  background-color: #ccc;
  color: var(--background);
}

.editor__title-input::placeholder {
  color: #444;
}

.editor__end {
  padding: 10px 20px;
  background-color: var(--background);
  color: #ddd;
}

.input-group {
  width: 100%;
  padding: 10px 0;
  display: flex;
  flex-direction: column;
  border-radius: 5px;
}

.input-group__section {
  /* width: 100%; */
  display: flex;
  align-items: center;
}

.input-group__checkbox {
  margin-right: 10px;
  cursor: pointer;
}

.input-group__label {
  margin-right: 8px;
  width: 130px;
  cursor: pointer;
}

.input-group__input {
  width: 80%;
  padding: 4px;
  border-radius: 4px;
  background-color: #ccc;
  color: var(--background);
}

.input-group__input:disabled {
  background-color: #888;
}

.input-group__btn {
  margin: 0 10px;
  padding: 5px 16px;
  border: 1px solid rgb(99, 96, 96);
  color: rgb(99, 96, 96);
  border-radius: 8px;
  background-color: transparent;
  cursor: pointer;
}

.input-group__btn.active {
  border: 1px solid var(--primary);
  color: var(--primary);
}

.input-group__btn:disabled {
  opacity: 0.5;
}

.editor__farewell {
  position: relative;
  margin-top: 15px;
}

.editor__sender {
  margin-left: 20px;
  width: 300px;
}

.editor__send-btn {
  display: block;
  margin: 10px auto 0 auto;
  padding: 10px 20px;
  width: 200px;
  background-color: transparent;
  border: 1px solid var(--primary);
  color: var(--primary);
  border-radius: 5px;
  cursor: pointer;
}

@media (min-width: 520px) {
  .input-group {
    flex-direction: row;
    align-items: center;
  }

  .input-group__section.second {
    flex-grow: 1;
  }

  .input-group__input {
    width: 100%;
  }

  .editor__send-btn {
    margin: 10px 0 0 auto;
  }
}
</style>
