<template>
  <div class="about">
    <form action="#">
      Họ tên:
      <input type="text" name="firstName" v-model="fullName"  placeholder="..."/> <br/> <br/>
      Mã học sinh:
      <input v-model="studentCode" type="text" name="lastName" placeholder="..." /> <br/> <br/>

      <button @click="handleSubmit" style="background-color: rgb(43, 207, 71) ;" class="btnSearch">Tìm</button>
    </form>
    <xlsx-table :sheet="studentData" class="aa" />
    <p> {{ studentData?.[0]?.["Dân tộc"] }}</p>
    <table class="table table-striped custom-table" >
      <thead>
        <tr>
          <th scope="col">STT</th>
          <th scope="col">Trường</th>
          <th scope="col">Quận/ Huyện</th>
          <th scope="col">Mã học sinh</th>
          <th scope="col"> Lớp </th>
          <th scope="col"> Họ tên </th>
          <th scope="col"> Ngày </th>
          <th scope="col"> Tháng </th>
          <th scope="col"> Năm </th> 
          <th scope="col"> Giới tính </th>
          <th scope="col"> Nơi sinh</th>
          <th scope="col"> Dân tộc </th>
          <th scope="col"> Hộ khẩu thường trú </th> 
          <th scope="col"> Điện thoại liên hệ</th>
          <th scope="col"> Tổng điểm năm lớp 1</th>
          <th scope="col"> Tổng điểm năm lớp 2</th>
          <th scope="col"> Tổng điểm năm lớp 3</th>
          <th scope="col"> Tổng điểm năm lớp 4</th>
          <th scope="col"> Tổng điểm năm lớp 5</th>
          <th scope="col"> Tổng 5 năm học </th>
          <th scope="col"> Điểm ưu tiên</th>
          <th scope="col"> Tổng điểm sơ tuyển </th>
          <th scope="col"> Ghi chú </th>
        </tr>
      </thead>
      <tbody>

        <tr v-for="(student, index) in studentData" :key="index">
          <th v-for="(data, index) in student" :key="index" scope="row">{{data}}</th>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
import axios from "axios";
export default {
  name: "HomeView",
  data() {
    return {
      fullName: "",
      studentCode: "",
      studentData: [],
    };
  },
  methods: {
    handleSubmit(e) {
      e.preventDefault();
      console.log(this.fullName);
      if (this.studentCode) {
        axios
          .get("http://localhost:3000/students?Mã học sinh=" + this.studentCode)
          .then((res) => {
            console.log(res);
            this.studentData = [...res.data];
          });
      } else {
        axios
          .get("http://localhost:3000/students?Họ tên=" + this.fullName)
          .then((res) => {
            console.log(res);
            this.studentData = [...res.data];
          });
      }
     
    },
  },
};
</script>

<style scoped>
  .custom-table{
    display: inline-block;
    max-width: 1200px;
    max-height: 400px;
    overflow-x: scroll;
    overflow-y: scroll;
    border: 1px solid #ccc;
    border-collapse: collapse;
    
  }

  .custom-table thead tr th, .custom-table thead tr td {
    min-width: 200px;
    border: 1px solid black;
    border-collapse: collapse;
    background-color: rgb(43, 207, 71);
  }
  
</style>
