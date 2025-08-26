<template>
  <div
    class="tile is-parent is-8-desktop Sidebar"
    :class="{ 'Sidebar--open': isOpen }"
  >
    <div class="tile is-child box is-success">
      <div class="tile is-child box is-success">
        <div class="content">
          <button class="delete is-large" @click="$emit('close')"></button>

          <!-- Edit Post Form -->
          <div v-if="editingPost">
            <h2 class="title is-4">
              {{ editingPost.id ? "Edit Post" : "Create Post" }}
            </h2>
            <div class="field">
              <label class="label">Title</label>
              <div class="control">
                <input
                  v-model="editingPost.title"
                  class="input"
                  type="text"
                  placeholder="Post title"
                />
              </div>
            </div>
            <div class="field">
              <label class="label">Content</label>
              <div class="control">
                <textarea
                  v-model="editingPost.body"
                  class="textarea"
                  placeholder="Post content"
                ></textarea>
              </div>
            </div>
            <div class="field is-grouped">
              <div class="control">
                <button
                  class="button is-primary"
                  @click="$emit('update-post', editingPost)"
                >
                  {{ editingPost.id ? "Update" : "Create" }}
                </button>
              </div>
              <div class="control">
                <button class="button is-light" @click="$emit('cancel-edit')">
                  Cancel
                </button>
              </div>
            </div>
          </div>

          <!-- Post Preview -->
          <div v-else-if="currentPost">
            <h2 class="title is-4">{{ currentPost.title }}</h2>
            <p class="content">{{ currentPost.body }}</p>

            <div class="field is-grouped">
              <div class="control">
                <button class="button is-info" @click="$emit('edit-post')">
                  Edit
                </button>
              </div>
              <div class="control">
                <button
                  class="button is-danger"
                  @click="$emit('delete-post', currentPost.id)"
                >
                  Delete
                </button>
              </div>
            </div>

            <!-- Comments Section will be here -->

            <div class="comments-section">
              <h3 class="title is-5">Comments</h3>
              <p>Comments functionality will be added here</p>
            </div>
          </div>

          <!-- Create Post Form -->
          <div v-else>
            <h2 class="title is-4">Create New Post</h2>
            <div class="field">
              <label class="label">Title</label>
              <div class="control">
                <input
                  v-model="newPost.title"
                  class="input"
                  type="text"
                  placeholder="Post title"
                />
              </div>
            </div>
            <div class="field">
              <label class="label">Content</label>
              <div class="control">
                <textarea
                  v-model="newPost.body"
                  class="textarea"
                  placeholder="Post content"
                ></textarea>
              </div>
            </div>
            <div class="field is-grouped">
              <div class="control">
                <button
                  class="button is-primary"
                  @click="$emit('create-post', newPost)"
                >
                  Create
                </button>
              </div>
              <div class="control">
                <button class="button is-light" @click="$emit('close')">
                  Cancel
                </button>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { ref, watch } from "vue";

export default {
  name: "Sidebar",
  props: {
    isOpen: Boolean,
    currentPost: Object,
    editingPost: Object,
  },
  emits: [
    "close",
    "create-post",
    "update-post",
    "delete-post",
    "edit-post",
    "cancel-edit",
  ],
  setup(props, { emit }) {
    const newPost = ref({
      title: "",
      body: "",
      userId: 1,
    });

    // Reset form when sidebar closes
    watch(
      () => props.isOpen,
      (isOpen) => {
        if (!isOpen) {
          newPost.value = { title: "", body: "", userId: 1 };
        }
      }
    );

    return {
      newPost,
    };
  },
};
</script>

<style scoped>
.Sidebar {
  overflow: hidden;
  opacity: 0;
  transition-property: max-width, opacity;
  transition-duration: 0.5s;
  transition-timing-function: ease-in-out;
}

@media (min-width: 769px) {
  .Sidebar {
    max-width: 0;
  }
}

.Sidebar--open {
  opacity: 1;
}

@media (min-width: 769px) {
  .Sidebar--open {
    max-width: 50%;
  }
}

.delete {
  position: absolute;
  top: 10px;
  right: 10px;
}

.content {
  position: relative;
  padding-top: 2rem;
}

.comments-section {
  margin-top: 2rem;
  padding-top: 1rem;
  border-top: 1px solid #dbdbdb;
}
</style>
