<template>
  <div class="modal-wrap">
    <div class="cocktail_detail_box">
      <div class="cocktail_detail_box_img">
        <img :src="cocktailDetail.imgURL" alt="Cocktail Image">
      </div>
      <div class="cocktail_detail_box_content">
        <h1>{{ cocktailDetail.kr_Name }}</h1>
        <p>Price: {{ cocktailDetail.price }}원</p>
        <p>Amount: {{ cocktailDetail.amount }}</p>
        <p>Alcohol: {{ cocktailDetail.alcohol }}</p>
        <p>Content: {{ cocktailDetail.content }}</p>
      </div>

      <div class="Detail_modal-btn">
        <button class="Detail_modal-btn1" @click="addCart">추가하기</button>
        <button class="Detail_modal-btn2" @click="close">돌아가기</button>
     </div>

    </div>
    
  </div>
</template>

   
<script>
import { useCartStore } from "@/stores/cart";
import axios from 'axios';

export default {
  data() {
    return {
      CocktailSeq: useCartStore().tempSeq,
      cocktailDetail: {},
    }
  },
  methods: {
    addCart() {
      console.log("cocktail Seq " + this.CocktailSeq)
      useCartStore().addToCart(this.CocktailSeq)
      this.$router.push('/cocktail/main');
    },
    close() {
      this.$router.push('/cocktail/main');
    },
    async getCocktailData(seq) {
      try {
        const response = await axios.get(`http://3.38.22.113:8080/api/v1/cocktail/${seq}`, {});

        // 서버 응답 성공
        if (response.status === 200) {
          const cocktailDetailData = response.data.cocktailDetail;
          this.cocktailDetail = cocktailDetailData; // Set the cocktailDetail property
        } else {
          alert('Login failed. Please check your credentials.');
          console.error('Unexpected response status or login failure:', response.status);
        }
      } catch (error) {
        alert('An error occurred during the request. Please try again.');
        console.error('An error occurred during the request:', error);
      }
    },
  },
  mounted() {
    this.getCocktailData(useCartStore().tempSeq);
  },
};
</script>
   <style>
   /* dimmed */
 .modal-wrap {
   width: 100%;
   height: 100%;
   background: rgba(0, 0, 0, 0.4);
 }

 .cocktail_detail_box {
   position: relative;
   top: 50%;
   left: 50%;
   transform: translate(-50%, -50%);
   width: 80em;
   height: 50em;
   background: #ffffff;
   border-radius: 10px;
   box-sizing: border-box;
   padding-top: 5em;
   display: flex;
   
 }
 .cocktail_detail_box_img{
  height: 70%;
  width: 50%;
  margin-left: 1rem;
  display: flex;
  justify-content: center;
 }
 .cocktail_detail_box_content{
  height: 70%;
  width: 20%;
  display: flex;
  justify-content: center;
  flex-direction: column;
  margin-left: 15rem;
  border : 1px solid #ccc;
  background-color: #dee2e6;
 }
 .Detail_modal-btn{
  position: absolute;
  bottom: 5em; /* 원하는 위치로 조정 */
  left: 50%;
  transform: translateX(-50%);
  margin-top: 6rem;
 }
 
 .Detail_modal-btn > button{
     width: 10rem;
     height: 3rem;
     border-radius: 10px;
 }
 .Detail_modal-btn1{
  background-color: #ff7f7f;
 }
 .Detail_modal-btn2{
     margin-left: 10em;
     background-color: #c0c0c0;
 }
 </style>  