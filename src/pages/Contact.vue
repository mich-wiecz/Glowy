<template>
  <Layout>
    <div class="container">
      <section class="section">
        <Paragraph>
          <List
            :items="[
              'Do you have a job offer?',
              'Maybe you have an idea for an application / tool that you would like to see on the web?',
              'Maybe we can create something together?',
            ]"
          />
          <div class="contacts">
            <ContactItem
              class="contacts__item"
              v-for="{ iconURL, value, type } in contactData"
              :key="iconURL"
              :iconURL="iconURL"
              :value="value"
              :type="type"
            />
          </div>
        </Paragraph>
      </section>
      <MessageSender class="message-sender" />
    </div>
  </Layout>
</template>

<script>
import Paragraph from "../components/Paragraph.vue";
import List from "../components/List.vue";
import ContactItem from "../components/ContactItem.vue";
import Title from "../components/Title.vue";
import MessageSender from "../components/MessageSender.vue";

export default {
  metaInfo: {
    title: "Contact",
  },
  components: {
    Paragraph,
    List,
    ContactItem,
    Title,
    MessageSender,
  },
  data() {
    return {
      contactData: [
        {
          iconURL: "phone.svg",
          value: "698 419 296",
          type: "text",
        },
        {
          iconURL: "mail.svg",
          value: "m1998.wieczorek@gmail.com",
          type: "text",
        },
        {
          iconURL: "github.svg",
          value: "https://github.com/mich-wiecz",
          type: "link",
        },
      ],
    };
  },
};
</script>

<style scoped>
.container {
  margin: 0 20px;
  /* height: 100%; */

  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: space-evenly;
}

.section {
  margin: 20px 0;
  max-width: 400px;
}

.contacts {
  margin-top: 10px;
}

.contacts__item {
  margin-bottom: 10px;
}

.message-sender {
  margin: 40px 20px;
  width: 100%;
  max-width: 600px;
}
</style>
