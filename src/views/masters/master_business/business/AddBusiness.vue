<template>
<div class="text-center">
    <a href="javascript:;" @click="headerFooterModalPreview = true" class="btn btn-primary mr-2">Add New Business</a>
</div>
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
                        Read Docs
                    </DropdownItem>
                </DropdownContent>
            </DropdownMenu>
        </Dropdown>
    </ModalHeader>
    <ModalBody class="grid grid-cols-12 gap-4 gap-y-3">
        <div class="col-span-12 sm:col-span-12">
            <label for="modal-form-1" class="form-label">Business Name : </label>
            <input id="modal-form-1" type="text" class="form-control" placeholder="Business Name" />
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
export default {
  data() {
    return {
      headerFooterModalPreview : false,
    };
  },

  mounted() {
  },
  methods: {
  },
};
</script>