<template>
    <div class="home">
      <div class="hero">
        <h1>환영합니다!</h1>
        <p>ToDo 리스트 페이지에 오신 것을 환영합니다.</p>
        <img src="@/assets/duck.gif" alt="오둥이" />
        <p>{{ randomQuote }}</p>
        <router-link to="/TodoList" class="button">일정관리 시작하기</router-link>
      </div>
    </div>
  </template>
  
  <script>
  export default {
    data() {
      return {
        gifUrl: '',
        quotes: [
          "성공은 준비된 자에게 돌아옵니다.",
          "계획 없는 목표는 그냥 바람에 날려버리는 종이입니다.",
          "오늘 당신은 어제의 당신보다 나은 사람이 되기 위해 노력하세요.",
          "비폭력은 인류가 활용할 수 있는 가장 강력한 힘이다.",
          "강력한 이유는 강력한 행동을 낳는다.",
          "성공의 8할은 일단 출석하는 것이다."
        ]
      };
    },
    mounted() {
      this.getGif();
    },
    methods: {
      async getGif() {
        try {
          const response = await fetch('https://api.giphy.com/v1/gifs/random?api_key=YOUR_API_KEY&tag=inspiration&rating=g');
          const data = await response.json();
          this.gifUrl = data.data.image_url;
        } catch (error) {
          console.log(error);
        }
      }
    },
    computed: {
      randomQuote() {
        const randomIndex = Math.floor(Math.random() * this.quotes.length);
        return this.quotes[randomIndex];
      }
    }
  };
  </script>
  
  <style>
  .home {
    margin: 0 auto;
    padding: 50px;
    text-align: center;
  }
  
  .hero {
    background-color: #f2f2f2;
    padding: 30px;
    border-radius: 10px;
  }
  
  h1 {
    font-size: 36px;
    margin-bottom: 20px;
  }
  
  p {
    font-size: 18px;
    margin-bottom: 30px;
  }
  
  img {
    max-width: 300px;
    margin-bottom: 20px;
  }
  
  .button {
    display: inline-block;
    padding: 10px 20px;
    background-color: #1324bb85;
    color: #fff;
    text-decoration: none;
    border-radius: 5px;
    font-size: 16px;
  }
  
  .button:hover {
    background-color: #560ec9;
  }
  </style>
  