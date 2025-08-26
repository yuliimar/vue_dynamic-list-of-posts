<template>
  <div class="comment-form">
    <h4 class="title is-6">Add Comment</h4>

    <form @submit.prevent="submitComment">
      <div class="field">
        <label class="label">Name</label>
        <div class="control">
          <input
            v-model="form.name"
            class="input"
            type="text"
            placeholder="Your name"
            required
          />
        </div>
      </div>

      <div class="field">
        <label class="label">Email</label>
        <div class="control">
          <input
            v-model="form.email"
            class="input"
            type="email"
            placeholder="Your email"
            required
          />
        </div>
      </div>

      <div class="field">
        <label class="label">Comment</label>
        <div class="control">
          <textarea
            v-model="form.body"
            class="textarea"
            placeholder="Write your comment here..."
            required
          ></textarea>
        </div>
      </div>

      <div class="field is-grouped">
        <div class="control">
          <button
            type="submit"
            class="button is-primary"
            :class="{ 'is-loading': loading }"
          >
            Submit
          </button>
        </div>
        <div class="control">
          <button
            type="button"
            class="button is-light"
            @click="$emit('cancel')"
          >
            Cancel
          </button>
        </div>
      </div>
    </form>
  </div>
</template>

<script>
import { ref } from "vue";

export default {
  name: "CommentForm",
  props: {
    postId: {
      type: Number,
      required: true,
    },
  },
  emits: ["add-comment", "cancel"],
  setup(props, { emit }) {
    const form = ref({
      name: "",
      email: "",
      body: "",
    });

    const loading = ref(false);

    const submitComment = async () => {
      loading.value = true;
      const commentData = {
        ...form.value,
        postId: props.postId,
      };
      emit("add-comment", commentData);
      // Форма очищається в батьківському компоненті після успішного додавання
    };

    return {
      form,
      loading,
      submitComment,
    };
  },
};
</script>

<style scoped>
.comment-form {
  margin-top: 1.5rem;
  padding: 1rem;
  border: 1px solid #dbdbdb;
  border-radius: 4px;
  background: #f5f5f5;
}
</style>
