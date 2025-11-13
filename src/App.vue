<template>
  <div id="app">
    <!-- 헤더 -->
    <header v-if="isLoggedIn">
      <div class="header-top">
        <div class="logo">
          <h1>YeonPlay</h1>
        </div>
        <div class="user-info">
          <!-- 로그인 상태에 따라 닉네임 표시 -->
          <span>{{ loggedInUser }}님</span>
          <button v-if="isLoggedIn" @click="logout" class="logout-btn">로그아웃</button>
        </div>
      </div>
      <div class="header-bottom">
        <nav>
          <router-link to="/">홈</router-link>
          <router-link to="/popular">대세 콘텐츠</router-link>
          <router-link to="/now_playing">현재 상영 중인 영화</router-link>
          <router-link to="/genres">장르별 영화</router-link>
          <router-link to="/wishlist">내가 찜한 콘텐츠</router-link>
        </nav>
        <div class="search-container">
          <input
            type="text"
            v-model="searchQuery"
            placeholder="영화 제목을 입력하세요."
            @keyup.enter="performSearch"
          />
          <button @click="performSearch">검색!</button>
        </div>
      </div>
    </header>
    <main>
      <router-view @login-success="checkLoginStatus" />
    </main>
  </div>
</template>

<script>
export default {
  data() {
    return {
      searchQuery: "",
      isLoggedIn: false,
      loggedInUser: "", // 닉네임을 표시하기 위한 변수
    };
  },
  methods: {
    performSearch() {
      if (this.searchQuery.trim()) {
        this.$router.push({ name: "SearchMovies", query: { q: this.searchQuery } });
        this.searchQuery = "";
      } else {
        alert("검색어를 입력해주세요.");
      }
    },
    logout() {
      localStorage.removeItem("isLoggedIn");
      localStorage.removeItem("loggedInUser");
      this.isLoggedIn = false;
      this.loggedInUser = "";
      alert("로그아웃되었습니다.");
      this.$router.push("/signin");
    },
    checkLoginStatus() {
      this.isLoggedIn = localStorage.getItem("isLoggedIn") === "true";
      this.loggedInUser = localStorage.getItem("loggedInUser") || ""; // 닉네임 가져오기
      console.log("로그인 상태:", this.isLoggedIn, "닉네임:", this.loggedInUser);
    },
  },
  created() {
    this.checkLoginStatus();
  },
  watch: {
    $route() {
      this.checkLoginStatus();
    },
  },
};
</script>

<style>
/* 기본 스타일 */
#app {
  display: flex;
  flex-direction: column;
  justify-content: flex-start; /* 헤더가 상단에 고정 */
  align-items: stretch; /* 가로 공간 채우기 */
  min-height: 100vh;
}

/* 헤더 스타일 */
header {
  padding: 10px 20px;
  background-color: #2c3e50;
  color: white;
  box-sizing: border-box;
  display: flex;
  flex-direction: column; /* 상단과 하단을 수직으로 배치 */
  gap: 10px; /* 상단과 하단 사이 간격 */
}

/* 상단 로고와 사용자 정보 */
.header-top {
  display: flex;
  justify-content: space-between; /* 로고는 왼쪽, 사용자 정보는 오른쪽 */
  align-items: center;
}

.logo h1 {
  margin: 0;
  font-size: 1.8rem;
}

.user-info {
  display: flex;
  align-items: center;
  gap: 10px;
}

.logout-btn {
  padding: 5px 10px;
  background-color: #e74c3c;
  color: white;
  border: none;
  border-radius: 5px;
  cursor: pointer;
}

.logout-btn:hover {
  background-color: #c0392b;
}

/* 하단 메뉴와 검색 */
.header-bottom {
  display: flex;
  justify-content: space-between; /* 메뉴는 왼쪽, 검색은 오른쪽 */
  align-items: center;
  flex-wrap: wrap;
  gap: 10px;
}

/* 메뉴 박스 스타일 */
nav {
  display: flex;
  gap: 10px;
}

nav a {
  display: inline-block;
  padding: 8px 18px;
  background-color: #34495e;
  color: white;
  border-radius: 12px;
  text-decoration: none;
  font-size: 1rem;
  font-weight: 500;
  transition: all 0.3s ease;
}

nav a:hover {
  background-color: #1E90FF;
  color: white;
  transform: translateY(-2px);
  box-shadow: 0 4px 10px rgba(0,0,0,0.2);
}

nav a.router-link-exact-active {
  background-color: #1E90FF;
  color: white;
  font-weight: 600;
  transform: translateY(-2px);
  box-shadow: 0 4px 12px rgba(0,0,0,0.25);
}

/* 반응형 */
@media (max-width: 768px) {
  .header-bottom {
    flex-direction: column;
    align-items: flex-start;
  }

  nav {
    width: 100%;
    justify-content: flex-start;
    flex-wrap: wrap;
  }

  nav a {
    margin-bottom: 5px;
  }

  .search-container {
    width: 100%;
  }

  .search-container input {
    width: 100%;
    max-width: 300px;
  }
}

</style>
