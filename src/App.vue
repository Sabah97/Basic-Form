<template>
  <div class="pb-6 pt-6 md:mt-12">
    <div class="container flex flex-col md:w-7/12 p-16">
      <Form @submit="onSubmit">
        <div class="flex flex-col">
          <div
            class="drop-zone"
            @drop="onDrop($event, 1)"
            @dragenter.prevent
            @dragover.prevent
          >
            <div
              class="bg-white"
              v-for="item in getList(1)"
              :key="item.id"
              draggable="true"
              @dragstart="startDrag($event, item)"
            >
              <p class="mt-6 text-black font-semibold tracking-wide mr-4">
                Name <span class="text-red-600"> * </span>
              </p>
              <Field
                v-model="name"
                placeholder="Name"
                name="name"
                type="name"
                :rules="validateName"
                class="mt-4 w-9/12 text-black placeholder-gray-500 bg-transparent border-2 px-4 py-2 rounded-sm tracking-wide"
              />
              <ErrorMessage class="mt-4" name="name" />
            </div>
          </div>
          <div
            class="drop-zone"
            @drop="onDrop($event, 2)"
            @dragenter.prevent
            @dragover.prevent
          >
            <div
              class="bg-white"
              v-for="item in getList(2)"
              :key="item.id"
              draggable="true"
              @dragstart="startDrag($event, item)"
            >
              <p class="text-black font-semibold tracking-wide mr-4">
                Email <span class="text-red-600"> * </span>
              </p>

              <Field
                v-model="email"
                placeholder="Email"
                name="email"
                type="email"
                :rules="validateEmail"
                class="mt-4 w-9/12 text-black placeholder-gray-500 bg-transparent border-2 px-4 py-2 rounded-sm tracking-wide"
              />
              <ErrorMessage class="mt-4" name="email" />
            </div>
          </div>
          <div
            class="drop-zone"
            @drop="onDrop($event, 3)"
            @dragenter.prevent
            @dragover.prevent
          >
            <div
              class="bg-white"
              v-for="item in getList(3)"
              :key="item.id"
              draggable="true"
              @dragstart="startDrag($event, item)"
            >
              <p class="text-black font-semibold tracking-wide mr-4">
                Date of Birth <span class="text-red-600"> * </span>
              </p>

              <Field
                v-model="date"
                placeholder="YYYY-MM-DD"
                name="date"
                type="date"
                :rules="validateDate"
                class="mt-4 w-9/12 text-black placeholder-gray-500 bg-transparent border-2 px-4 py-2 rounded-sm tracking-wide"
              />
              <ErrorMessage class="mt-4" name="date" />
            </div>
          </div>
          <div
            class="drop-zone"
            @drop="onDrop($event, 4)"
            @dragenter.prevent
            @dragover.prevent
          >
            <div
              class="bg-white"
              v-for="item in getList(4)"
              :key="item.id"
              draggable="true"
              @dragstart="startDrag($event, item)"
            >
              <p class="text-black font-semibold tracking-wide mr-4">
                Bio <span class="text-red-600"> * </span>
              </p>

              <textarea
                v-model="bio"
                :rules="validateBio"
                @keyup="charCount()"
                required
                placeholder="Your Bio"
                name="bio"
                type="text"
                class="mt-4 w-9/12 text-black placeholder-gray-500 bg-transparent border-2 px-4 py-12 rounded-sm tracking-wide"
              />

              <p class="text-black font-medium mt-2">
                {{ totalcharacter }} characters
              </p>
              <p v-if="charError">{{ charError }}</p>
              <p v-if="error">{{ error }}</p>
              <ErrorMessage class="mt-4" name="bio" />
            </div>
          </div>
          <div
            class="drop-zone"
            @drop="onDrop($event, 5)"
            @dragenter.prevent
            @dragover.prevent
          >
            <div
              class="bg-white"
              v-for="item in getList(5)"
              :key="item.id"
              draggable="true"
              @dragstart="startDrag($event, item)"
            >
              <p class="text-black font-semibold tracking-wide mr-4">
                Image <span class="text-red-600"> * </span>
              </p>
              <div class="fle flex-col md:flex-row mt-2">
                <input
                  :rules="validateImg"
                  type="file"
                  id="imageUpload"
                  v-on:change="updatePreview"
                  accept=".png, .jpg,.jpeg"
                  :maxFileSize="500000"
                />
                <button
                  class="bg-blue-500 lg:w-3/12 px-4 lg:mt-0 mt-2 py-1 rounded-sm"
                >
                  <p class="text-white text-lg text-center font-semibold">
                    Upload
                  </p>
                </button>
                <ErrorMessage class="mt-4" name="imagePreview" />
                <p v-if="errorImg">{{ errorImg }}</p>
              </div>
              <img
                v-bind:src="imagePreview"
                class="image-preview"
                v-on:click="openUpload"
              />
            </div>
          </div>

          <button
            @click="handleSubmit"
            class="bg-blue-500 w-9/12 px-4 py-2 flex-row mt-12 rounded-sm"
          >
            <p class="text-white text-lg text-center font-semibold">Submit</p>
          </button>
        </div>
      </Form>
    </div>
  </div>
</template>
<script>
import { Form, Field, ErrorMessage } from "vee-validate";
import { ref } from "vue";
export default {
  setup() {
    const items = ref([
      { id: 0, title: "Name", list: 1 },
      { id: 1, title: "Email", list: 2 },
      { id: 2, title: "Date Of Birth", list: 3 },
      { id: 3, title: "Bio", list: 4 },
      { id: 4, title: "Image", list: 5 },
    ]);
    const getList = (list) => {
      return items.value.filter((item) => item.list == list);
    };
    const startDrag = (event, item) => {
      console.log(item);
      event.dataTransfer.dropEffect = "move";
      event.dataTransfer.effectAllowed = "move";
      event.dataTransfer.setData("itemID", item.id);
    };
    const onDrop = (event, list) => {
      console.log(list);
      const itemID = event.dataTransfer.getData("itemID");
      console.log("itemID", itemID);
      const item = items.value.find((item) => item.id == itemID);
      item.list = list;
      console.log("list", list);
    };

    return {
      getList,
      onDrop,
      startDrag,
    };
  },

  components: {
    Form,
    Field,
    ErrorMessage,
  },

  data() {
    return {
      message: "",
      totalcharacter: 0,
      imagePreview: "",
      name: "",
      email: "",
      date: "",
      bio: "",
      charError: "",
      error: "",
    };
  },
  methods: {
    onSubmit(values) {
      console.log("value", values, null, 2);
    },
    validateEmail(value) {
      // if the field is empty
      if (!value) {
        return "This field is required";
      }
      // if the field is not a valid email
      const regex = /^[A-Z0-9._%+-]+@[A-Z0-9.-]+\.[A-Z]{2,4}$/i;
      if (!regex.test(value)) {
        return "This field must be a valid email";
      }
      // All is good
      return true;
    },
    validateName(value) {
      // if the field is empty
      if (!value) {
        return "This field is required";
      }

      const regex = /^[a-z][a-z\s]*$/;
      if (!regex.test(value)) {
        return "This field must include alphabet";
      }
      // All is good
      return true;
    },
    validateBio(value) {
      // if the field is empty
      if (!value) {
        return "This field is required and max limit 5000";
      }

      // All is good
      return true;
    },
    validateDate(value) {
      // if the field is empty
      if (!value) {
        return "This field is required";
      }

      const regex =
        /^\d{4}-(02-(0[1-9]|[12][0-9])|(0[469]|11)-(0[1-9]|[12][0-9]|30)|(0[13578]|1[02])-(0[1-9]|[12][0-9]|3[01]))$/;
      if (!regex.test(value)) {
        return "This field must include a proper YYYY-MM-DD";
      }
      // All is good
      return true;
    },
    charCount: function () {
      this.totalcharacter = this.bio.length;
    },

    openUpload() {
      document.getElementById("imageUpload").click();
    },
    updatePreview(e) {
      var reader,
        files = e.target.files;
      if (files.length === 0) {
        console.log("empty");
      }
      reader = new FileReader();
      reader.onload = (e) => {
        this.imagePreview = e.target.result;
      };
      reader.readAsDataURL(files[0]);
    },
    handleSubmit() {
      console.log(this.name);
      console.log(this.email);
      console.log(this.bio);
      if (this.bio.length > 5000) {
        this.charError = "max limit 5000";
      }
      if (this.bio == "") {
        this.error = "This field required";
      }
      if (this.imagePreview == "") {
        this.errorImg = "This field is required";
      }
      console.log(this.date);
    },
  },
};
</script>
<style>
.drop-zone {
  width: 100%;
  background-color: #ffffff;
  margin: 50px auto;
  padding: 10px;
  min-height: 10px;
}
</style>
