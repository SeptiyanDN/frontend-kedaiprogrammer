<template>
  <div class="intro-y flex flex-col sm:flex-row items-center mt-8">
    <h2 class="text-lg font-medium mr-auto">Manage Business</h2>
    <div class="w-full sm:w-auto flex mt-4 sm:mt-0">
<AddService></AddService>

    </div>
  </div>
  <div class="intro-y box p-5 mt-5">
    <div class="flex flex-col sm:flex-row sm:items-end xl:items-start">
      <form id="tabulator-html-filter-form" class="xl:flex sm:mr-auto">
        <div class="sm:flex items-center sm:mr-4">
          <label class="w-12 flex-none xl:w-auto xl:flex-initial mr-2"
            >Field</label
          >
          <select
            id="tabulator-html-filter-field"
            v-model="filter.field"
            class="form-select w-full sm:w-32 2xl:w-full mt-2 sm:mt-0 sm:w-auto"
          >
            <option value="service_name">Service Name</option>
            <option value="business_name">Business Name</option>
          </select>
        </div>
        <div class="sm:flex items-center sm:mr-4 mt-2 xl:mt-0">
          <label class="w-12 flex-none xl:w-auto xl:flex-initial mr-2"
            >Type</label
          >
          <select
            id="tabulator-html-filter-type"
            v-model="filter.type"
            class="form-select w-full mt-2 sm:mt-0 sm:w-auto"
          >
            <option value="like">like</option>
            <option value="=">=</option>
            <option value="!=">!=</option>
          </select>
        </div>
        <div class="sm:flex items-center sm:mr-4 mt-2 xl:mt-0">
          <label class="w-12 flex-none xl:w-auto xl:flex-initial mr-2"
            >Value</label
          >
          <input
            id="tabulator-html-filter-value"
            v-model="filter.value"
            type="text"
            class="form-control sm:w-40 2xl:w-full mt-2 sm:mt-0"
            placeholder="Search..."
          />
        </div>
        <div class="mt-2 xl:mt-0">
          <button
            id="tabulator-html-filter-go"
            type="button"
            class="btn btn-primary w-full sm:w-16"
            @click="onFilter"
          >
            Go
          </button>
          <button
            id="tabulator-html-filter-reset"
            type="button"
            class="btn btn-secondary w-full sm:w-16 mt-2 sm:mt-0 sm:ml-1"
            @click="onResetFilter"
          >
            Reset
          </button>
        </div>
      </form>
      <div class="flex mt-5 sm:mt-0">
        <button
          id="tabulator-print"
          class="btn btn-outline-secondary w-1/2 sm:w-auto mr-2"
          @click="onPrint"
        >
          <PrinterIcon class="w-4 h-4 mr-2" /> Print
        </button>
        <Dropdown class="w-1/2 sm:w-auto">
          <DropdownToggle class="btn btn-outline-secondary w-full sm:w-auto">
            <FileTextIcon class="w-4 h-4 mr-2" /> Export
            <ChevronDownIcon class="w-4 h-4 ml-auto sm:ml-2" />
          </DropdownToggle>
          <DropdownMenu class="w-40">
            <DropdownContent>
              <DropdownItem @click="onExportCsv">
                <FileTextIcon class="w-4 h-4 mr-2" /> Export CSV
              </DropdownItem>
              <DropdownItem @click="onExportJson">
                <FileTextIcon class="w-4 h-4 mr-2" /> Export JSON
              </DropdownItem>
              <DropdownItem @click="onExportXlsx">
                <FileTextIcon class="w-4 h-4 mr-2" /> Export XLSX
              </DropdownItem>
              <DropdownItem @click="onExportHtml">
                <FileTextIcon class="w-4 h-4 mr-2" /> Export HTML
              </DropdownItem>
            </DropdownContent>
          </DropdownMenu>
        </Dropdown>
      </div>
    </div>
    <div class="overflow-x-auto scrollbar-hidden">
      <div
        id="tabulator"
        ref="tableRef"
        class="mt-5 table-report table-report--tabulator"
      ></div>
    </div>
  </div>
  <!-- END: HTML Table Data -->
</template>

<script setup>
import { ref, reactive, onMounted } from "vue";
import * as xlsx from 'xlsx';
import { createIcons, icons } from "lucide";
import Tabulator from "tabulator-tables";
import dom from "@left4code/tw-starter/dist/js/dom";
import AddService from "./AddService.vue"

const tableRef = ref();
const tabulator = ref();

const filter = reactive({
  field: "service_name",
  type: "like",
  value: "",
},
);

const initTabulator = () => {

  tabulator.value = new Tabulator(tableRef.value, {
    ajaxURL: `${process.env.VITE_API_URL}/services/list`,
    ajaxConfig:{
      headers: {
        "Content-Type": "application/json",
        Authorization: `Bearer ${localStorage.getItem("token")}`,
      }
    },
    
    ajaxFiltering: true,
    ajaxSorting: true,
    printAsHtml: true,
    printStyled: true,
    pagination:"remote", // menggunakan pagination berbentuk scroll
    paginationSize: 5,
    paginationSizeSelector: [5, 10, 15, 25],
    layout: "fitColumns",
    responsiveLayout: "collapse",
    placeholder: "No matching records found",
    
    columns: [
      {
        formatter: "responsiveCollapse",
        width: 40,
        minWidth: 30,
        hozAlign: "center",
        resizable: false,
        headerSort: false,
      },

      // For HTML table
      {
        title: "Service Name",
        minWidth: 200,
        responsive: 0,
        field: "service_name",
        vertAlign: "middle",
        print: false,
        download: false,
        formatter(cell) {
          return `<div>
                <div class="font-medium whitespace-nowrap">${
                  cell.getData().service_name
                }</div>
              </div>`;
        },
      },
      {
        title: "Business Name",
        minWidth: 200,
        responsive: 0,
        field: "business_name",
        vertAlign: "middle",
        print: false,
        download: false,
        formatter(cell) {
          return `<div>
                <div class="font-medium whitespace-nowrap">${
                  cell.getData().business_name
                }</div>
              </div>`;
        },
      },
      {
        title: "STATUS",
        minWidth: 200,
        field: "is_active",
        hozAlign: "center",
        vertAlign: "middle",
        print: false,
        download: false,
        formatter(cell) {
          return `<div class="flex items-center lg:justify-center ${
            cell.getData().is_active ? "text-success" : "text-danger"
          }">
                <i data-lucide="check-square" class="w-4 h-4 mr-2"></i> ${
                  cell.getData().is_active ? "Active" : "Inactive"
                }
              </div>`;
        },
      },
      {
        title: "ACTIONS",
        minWidth: 200,
        field: "actions",
        responsive: 1,
        hozAlign: "center",
        vertAlign: "middle",
        print: false,
        download: false,
        formatter() {
          const a = dom(`<div class="flex lg:justify-center items-center">
                <a class="flex items-center mr-3" href="javascript:;">
                  <i data-lucide="check-square" class="w-4 h-4 mr-1"></i> Edit
                </a>
                <a class="flex items-center text-danger" href="javascript:;">
                  <i data-lucide="trash-2" class="w-4 h-4 mr-1"></i> Delete
                </a>
              </div>`);
          dom(a).on("click", function () {
            // On click actions
          });

          return a[0];
        },
      },

      // For print format
      {
        title: "Business Name",
        field: "business_name",
        visible: false,
        print: true,
        download: true,
      },
      {
        title: "Business Domain",
        field: "domain",
        visible: false,
        print: true,
        download: true,
      },
      {
        title: "Status",
        field: "is_active",
        visible: false,
        print: true,
        download: true,
        formatterPrint(cell) {
          return cell.getValue() ? "Active" : "Inactive";
        },
      },
    ],
    renderComplete() {
      createIcons({
        icons,
        "stroke-width": 1.5,
        nameAttr: "data-lucide",
      });
    },
  });
  
};

// Redraw table onresize
const reInitOnResizeWindow = () => {
  window.addEventListener("resize", () => {
    tabulator.value.redraw();
    createIcons({
      icons,
      "stroke-width": 1.5,
      nameAttr: "data-lucide",
    });
  });
};

// Filter function
const onFilter = () => {
  tabulator.value.setFilter(filter.field, filter.type, filter.value);
};

// On reset filter
const onResetFilter = () => {
  filter.field = "service_name";
  filter.type = "like";
  filter.value = "";
  onFilter();
};

// Export
const onExportCsv = () => {
  tabulator.value.download("csv", "data.csv");
};

const onExportJson = () => {
  tabulator.value.download("json", "data.json");
};

const onExportXlsx = () => {
  const win = window;
  win.XLSX = xlsx;
  tabulator.value.download("xlsx", "data.xlsx", {
    sheetName: "Products",
  });
};

const onExportHtml = () => {
  tabulator.value.download("html", "data.html", {
    style: true,
  });
};

// Print
const onPrint = () => {
  tabulator.value.print();
};

onMounted(() => {
  initTabulator();
  reInitOnResizeWindow();
});
</script>
