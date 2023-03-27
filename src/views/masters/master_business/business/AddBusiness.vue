<template>
    <!-- Add Modal -->
<!-- BEGIN: Modal Toggle -->
<div class="text-center">
    <a href="javascript:;" @click="headerFooterModalPreview = true" class="btn btn-primary mr-2">Add New Business</a>
</div>
<!-- END: Modal Toggle -->
<!-- BEGIN: Modal Content -->
<Modal size="modal-xl" :show="headerFooterModalPreview" @hidden="headerFooterModalPreview = false" >
    <ModalHeader>
        <h2 class="font-medium text-base mr-auto">
            Add New Business
        </h2>
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
                        Download Docs
                    </DropdownItem>
                </DropdownContent>
            </DropdownMenu>
        </Dropdown>
    </ModalHeader>
    <ModalBody class="grid grid-cols-12 gap-4 gap-y-3">
        <div class="col-span-12 sm:col-span-6">
            <label for="modal-form-1" class="form-label">From</label>
            <input id="modal-form-1" type="text" class="form-control" placeholder="example@gmail.com" />
        </div>
        <div class="col-span-12 sm:col-span-6">
            <label for="modal-form-2" class="form-label">To</label>
            <input id="modal-form-2" type="text" class="form-control" placeholder="example@gmail.com" />
        </div>
        <div class="col-span-12 sm:col-span-6">
            <label for="modal-form-3" class="form-label">Subject</label>
            <input id="modal-form-3" type="text" class="form-control" placeholder="Important Meeting" />
        </div>
        <div class="col-span-12 sm:col-span-6">
            <label for="modal-form-4" class="form-label">Has the Words</label>
            <input id="modal-form-4" type="text" class="form-control" placeholder="Job, Work, Documentation" />
        </div>
        <div class="col-span-12 sm:col-span-6">
            <label for="modal-form-5" class="form-label">Doesn't Have</label>
            <input id="modal-form-5" type="text" class="form-control" placeholder="Job, Work, Documentation" />
        </div>
        <div class="col-span-12 sm:col-span-6">
            <label for="modal-form-6" class="form-label">Size</label>
            <select id="modal-form-6" class="form-select">
                <option>10</option>
                <option>25</option>
                <option>35</option>
                <option>50</option>
            </select>
        </div>
    </ModalBody>
    <ModalFooter>
        <button type="button" @click="headerFooterModalPreview = false" class="btn btn-outline-secondary w-20 mr-1">
            Cancel
        </button>
        <button type="button" class="btn btn-primary w-20">
            Send
        </button>
    </ModalFooter>
</Modal>
<!-- END: Modal Content -->
<!-- End Add Modal  -->
</template>

<script>
import axios from "axios";
export default {
  data() {
    return {
      headerFooterModalPreview : false,
    };
  },

  mounted() {
    // this.getBusiness();
  },
  methods: {
    // getBusiness() {
    //   axios.get("https://backend.septiyan.my.id/api/v1/business/list",{
    //     headers: {
    //       "Content-Type": "application/json",
    //       "Authorization" : `Bearer ${localStorage.getItem("token")}`
    //     },
    //   }).then((response) => {
    //     this.business = response.data.data;
    //   });
    // },
  },
};
</script>