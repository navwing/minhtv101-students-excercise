<template>
  <div class="hello">
     <img src="https://tracuutuyensinh.vn/wp-content/uploads/2020/11/logo-tracuuts-300x101.png" alt="No image" />
    <section>
      <input type="file" @change="onChange" />
      <XlsxWorkbook @created="onCreated" />
      <xlsx-read :file="file">
        <xlsx-sheets>
          <template #default="{ sheets }">
            <select v-model="selectedSheet">
              <option v-for="sheet in sheets" :key="sheet" :value="sheet">
                {{ sheet }}
              </option>
            </select>
          </template>
        </xlsx-sheets> 
        <xlsx-table :sheet="selectedSheet" class="aa" />
        <!-- <xlsx-json :sheet="selectedSheet">
          <template #default="{ collection }">
            <div>
              {{ collection }}
            </div>
          </template>
        </xlsx-json> -->
      </xlsx-read>
    </section>
  </div>
</template>
<script>
import {
  XlsxRead,
  XlsxTable,
  XlsxSheets,
  // XlsxJson,
  XlsxWorkbook,
  // XlsxSheet,
  // XlsxDownload,
} from "vue3-xlsx";
// import XLSX from "xlsx";
var XLSX = require("xlsx");
import axios from "axios";
export default {
  components: {
    XlsxRead,
    XlsxTable,
    XlsxSheets,
    // XlsxJson,
    XlsxWorkbook,
    // XlsxSheet,
    // XlsxDownload,
  },
  name: "HelloWorld",
  data() {
    return {
      file: null,
      selectedSheet: null,
      sheetName: null,
      sheets: [{ name: "SheetOne", data: [{ c: 2 }] }],
      collection: [{ a: 1, b: 2 }],
    };
  },
  methods: {
    onChange(event) {
      console.log("data", this.selectedSheet)
      const propNameArr = [
        "STT",
        "Trường Tiểu học",
        "Quận / Huyện",
        "Mã học sinh",
        "Lớp",
        "Họ tên",
        "Ngày",
        "Tháng",
        "Năm",
        "Giới tính",
        "Nơi sinh",
        "Dân tộc",
        "Hộ khẩu thường trú",
        "Điện thoại liên hệ",
        "Tổng điểm năm lớp 1",
        "Tổng điểm năm lớp 2",
        "Tổng điểm năm lớp 3",
        "Tổng điểm năm lớp 4",
        "Tổng điểm năm lớp 5",
        "Tổng điểm 5 năm",
        "Điểm ưu tiên",
        "Tổng điểm sơ tuyển",
        "Ghi chú",
      ];
      var reader = new FileReader();
      reader.onload = function (e) {
        console.log("xlsx: ", XLSX);
        console.log("reader", e);
        var data = new Uint8Array(e.target.result);
        console.log("Data: ", data);
        var workbook = XLSX.read(data, { type: "array" });
        console.log("workbook", workbook);
        let sheetName = workbook.SheetNames[0];
        console.log("result", sheetName);
        let worksheet = workbook.Sheets[sheetName];
        console.log("worksheet", worksheet);
        const finalData = XLSX.utils.sheet_to_json(worksheet);
        console.log("aâ", finalData);
        console.log("data", this.selectedSheet)
        finalData.map((data, index) => {
          const studentInfo = {};
          let i = 0;
          if (index >= 3) {
            for (let key in data) {
              studentInfo[propNameArr[i]] = data[key];
              i++;
            }
            console.log("student:", studentInfo);
            axios
              .post("http://localhost:7777/students", studentInfo)
              .then((res) => console.log(res))
              .catch((err) => console.log(err));
          }
        });
        this.file=null
        // đoạn xử lý data
      };
      console.log("reader", reader);

      console.log(event);
      this.file = event.target.files ? event.target.files[0] : null;
      reader.readAsArrayBuffer(this.file);
      console.log(this.file);
    },
    addSheet() {
      this.sheets.push({ name: this.sheetName, data: [...this.collection] });
      this.sheetName = null;
    }
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
.aa{
  border: 1px solid black;
}
</style>
