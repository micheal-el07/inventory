<!-- <template>
  <div>
    <table>
      <thead>
        <tr>
          <th v-for="header in headers" :key="header">{{ header }}</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(row, index) in tableData" :key="index">
          <td v-for="header in headers" :key="header">{{ row[header] }}</td>
        </tr>
      </tbody>
    </table>
    <button @click="exportToCSV">Export to CSV</button>
  </div>
</template> -->

<template>
  <div class="flex justify-center items-center min-h-screen bg-gray-100">
    <div class="overflow-auto">
      <table
        class="table-auto border-collapse border border-black min-w-[300px] mx-auto"
      >
        <thead>
          <tr>
            <th
              v-for="header in headers"
              :key="header"
              class="border border-black px-4 py-2 bg-gray-200 text-center"
            >
              {{ header }}
            </th>
          </tr>
        </thead>
        <tbody>
          <tr
            v-for="(row, index) in tableData"
            :key="index"
            class="min-h-[100px] border border-black"
          >
            <td
              v-for="header in headers"
              :key="header"
              class="border border-black px-4 py-2 text-center"
            >
              {{ row[header] }}
            </td>
          </tr>
        </tbody>
      </table>
      <div class="text-center mt-4">
        <button
          @click="exportToCSV"
          class="px-4 py-2 bg-blue-500 text-white rounded hover:bg-blue-600"
        >
          Export to CSV
        </button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      // Define table headers
      headers: ["Asset Name", "Department"],
      // Define table data
      tableData: [
        { "Asset Name": "Printer", Department: "HR" },
        { "Asset Name": "Monitor", Department: "IT" },
        { "Asset Name": "Barcode Scanner", Department: "ACCOUNT" },
      ],
    };
  },
  methods: {
    exportToCSV() {
      const csvContent = this.generateCSV(this.headers, this.tableData);

      const blob = new Blob([csvContent], { type: "text/csv;charset=utf-8;" });

      const link = document.createElement("a");
      const url = URL.createObjectURL(blob);
      link.setAttribute("href", url);
      link.setAttribute("download", "table_data.csv");

      document.body.appendChild(link);
      link.click();

      document.body.removeChild(link);
      URL.revokeObjectURL(url);
    },
    generateCSV(headers, data) {
      const headerRow = headers.join(",");

      const rows = data.map((row) =>
        headers.map((header) => `"${row[header] || ""}"`).join(",")
      );

      return [headerRow, ...rows].join("\n");
    },
  },
};
</script>
