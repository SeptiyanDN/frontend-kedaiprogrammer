<template>
    <div class="text-center">
      <a
        href="javascript:;"
        @click="headerFooterModalPreview = true"
        class="btn btn-primary mr-2"
      >
        Add New Categories
      </a>
    </div>
    <Modal
      size="modal-xl"
      :show="headerFooterModalPreview"
      @hidden="headerFooterModalPreview = false"
    >
      <ModalHeader>
        <h2 class="font-medium text-base mr-auto">Add New Business</h2>
        <button class="btn btn-outline-secondary hidden sm:flex">
          <FileIcon class="w-4 h-4 mr-2" /> Read Docs
        </button>
        <Dropdown class="sm:hidden">
          <DropdownToggle class="w-5 h-5 block" href="javascript:;">
            <MoreHorizontalIcon class="w-5 h-5 text-slate-500" />
          </DropdownToggle>
          <DropdownMenu class="w-40">
            <DropdownContent>
              <DropdownItem>
                <FileIcon class="w-4 h-4 mr-2" />
                Read Docs
              </DropdownItem>
            </DropdownContent>
          </DropdownMenu>
        </Dropdown>
      </ModalHeader>
      <ModalBody class="grid grid-cols-12 gap-4 gap-y-3">
        <div class="col-span-12 sm:col-span-12">
          <label for="modal-form-1" class="form-label">Categories Name : </label>
          <input
            id="modal-form-1"
            type="text"
            class="form-control"
            placeholder="example@gmail.com"
            v-model="category_name"
          />
        </div>
        <div class="col-span-12 sm:col-span-12">
          <label for="modal-form-2" class="form-label">Select Business Name : </label>
          <TomSelect v-model="selected_business_id" class="w-full">
            <option
              v-for="business in business"
              :key="business.id"
              :value="business.business_id"
              :disabled="business.is_active == false"
            >
              {{ business.business_name }}
            </option>
          </TomSelect>
        </div>
      </ModalBody>
      <ModalFooter>
        <button
          type="button"
          @click="headerFooterModalPreview = false"
          class="btn btn-outline-secondary w-20 mr-1"
        >
          Cancel
        </button>
        <button type="button" class="btn btn-primary w-20" @click="sendData">
          Send
        </button>
      </ModalFooter>
    </Modal>
  </template>
  
  <script>
  import axios from "axios";
  export default {
    data() {
      return {
        headerFooterModalPreview: false,
        business: [],
        category_name: "",
        selected_business_id: "",
      };
    },
  
    mounted() {
      this.getBusiness();
    },
  
    methods: {
      getBusiness() {
        axios
          .get(
            "https://backend.septiyan.my.id/api/v1/business/list",
            {
              headers: {
                "Content-Type": "application/json",
                Authorization: `Bearer ${localStorage.getItem("token")}`,
              },
            }
          )
          .then((response) => {
            this.business = response.data.data;
          });
      },
      sendData() {
        const dataToSend = {
          category_name: this.category_name,
          business_id: this.selected_business_id,
        };
        console.log(dataToSend);
        // send data to API here
        this.headerFooterModalPreview = false;
      },
    },
  };
  </script>