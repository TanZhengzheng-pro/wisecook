<template>
  <div id="app">
    <!-- 黄色色块 -->
    <div class="yellow-strip"></div>

    <!-- 顶部导航栏 -->
    <nav class="navbar">
      <!-- 左侧 LOGO -->
      <div class="navbar-left">
        <a href="/">
          <img class="logo" src="./assets/logo.png" alt="Logo" />
        </a>
      </div>

      <!-- 右侧导航标签和搜索框 -->
      <div class="navbar-right">
        <a href="#" class="nav-item">Home</a>
        <a href="#" class="nav-item">Recipe Roulette</a>
        <a href="#" class="nav-item">My Page</a>
        
        <!-- 搜索框 -->
        <div class="search-container">
          <button class="search-btn">
            <i class="fas fa-search"></i> <!-- 放大镜图标 -->
          </button>
          <input type="text" class="search-box" placeholder="Search" />
        </div>
      </div>

      <!-- 移动端导航按钮 -->
      <div class="navbar-toggle" @click="toggleNav">
        <span class="bar"></span>
        <span class="bar"></span>
        <span class="bar"></span>
      </div>
    </nav>

    <!-- 新增的图片和文字盒子 -->
    <div class="image-box">
      <div class="overlay"></div>
      <div class="text">
        <p>Just tell us what ingredients do you have and</p>
        <p>we will show you</p>
        <p>what can you make.</p>
      </div>
    </div>

    <!-- 页面主体内容 -->
    <main class="content">
  <h1>Which ingredients are available?</h1>
  <p>Start building your amazing app here.</p>

  <!-- 新增的组块 -->
  <div class="ingredients-container">
  <!-- 动态渲染 ingredient-item -->
  <div class="ingredient-item" v-for="(ingredient, index) in ingredients" :key="index">
    <img class="ingredient-image" :src="ingredient.image" :alt="ingredient.name" />
    <div class="ingredient-text">{{ ingredient.name }}</div>
    <!-- 动态生成下拉菜单组 -->
    <div class="dropdowns">
      <div v-for="(dropdownGroup, idx) in ingredient.dropdowns" :key="idx" class="dropdown-group">
        <select class="dropdown">
          <option>Option 1</option>
          <option>Option 2</option>
        </select>
        <select class="dropdown">
          <option>Option 1</option>
          <option>Option 2</option>
        </select>
      </div>
    </div>
    <!-- 控制按钮区域 -->
    <div class="control-buttons">
      <button @click="addDropdown(index)">+</button>
      <button @click="removeDropdown(index)">-</button>
    </div>

  </div>
</div>

    <!-- Match按钮区域 -->
    <div class="match-buttons">
      <button @click="addDropdown(index)">Partial Matching</button>
      <button @click="removeDropdown(index)">Strict Matching</button>
    </div>


  <!-- 新盒子 -->
  <div class="extra-box"></div>

 </main>

 <footer class="footer">
  <!-- 左侧 LOGO -->
  <div class="footer-left">
    <img class="footer-logo" src="./assets/logo.png" alt="Footer Logo" />
  </div>

  <!-- 右侧社交媒体 -->
  <div class="footer-right">
    <span class="connect-text">Connect With Us:</span>
    <div class="social-icons">
      <a href="https://facebook.com" target="_blank">
        <i class="fab fa-facebook-square"></i>
      </a>
      <a href="https://twitter.com" target="_blank">
        <i class="fab fa-twitter-square"></i>
      </a>
      <a href="https://instagram.com" target="_blank">
        <i class="fab fa-instagram-square"></i>
      </a>
    </div>
  </div>
</footer>


  </div>
</template>

<script>
export default {
  name: "App",
  data() {
    return {
      isNavOpen: false,
        // 食材列表，每个食材项包含 name, image 和 dropdowns 数组
        ingredients: [
        {
          name: "Meat & Eggs",
          image: require("@/assets/meat-and-eggs.jpg"),
          dropdowns: [{}, {}], // 初始有两组下拉菜单
        },
        {
          name: "Vegetables",
          image: require("@/assets/vegetables.jpg"),
          dropdowns: [{}, {}], // 初始有两组下拉菜单
        },
        {
          name: "Staple Foods",
          image: require("@/assets/staple-foods.jpg"),
          dropdowns: [{}, {}], // 初始有两组下拉菜单
        },
        {
          name: "Kitchenware",
          image: require("@/assets/kitchenware.jpg"),
          dropdowns: [{}, {}], // 初始有两组下拉菜单
        },


      ],
    };
  },
  methods: {

    // 添加一组下拉菜单
    addDropdown(index) {
      this.ingredients[index].dropdowns.push({}); // 向对应的食材添加一个新的下拉菜单组
    },

    // 删除最下方一组下拉菜单
    removeDropdown(index) {
      if (this.ingredients[index].dropdowns.length > 1) {
        this.ingredients[index].dropdowns.pop(); // 删除最末尾的下拉菜单组
      }
    },
  },

    toggleNav() {
      this.isNavOpen = !this.isNavOpen;
    },
  
};
</script>

<style>
/* 定义主题变量 */
:root {
  --theme-color: #FFDB63;
  --text-color: #2C2B2B;
}

/* 基本样式重置 */
body {
  margin: 0;
  font-family: Inter, sans-serif;
}

/* 黄色色块样式 */
.yellow-strip {
  width: 100%;
  height: 30px;
  background-color: var(--theme-color);
}

/* 顶部导航栏样式 */
.navbar {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 10px 40px;
  background-color: white;
  color: var(--text-color);
  box-shadow: 0 2px 5px rgba(0, 0, 0, 0.1);
  position: relative;
}

.navbar-left {
  display: flex;
  align-items: center;
}

.logo {
  height: 70px;
}

.navbar-right {
  display: flex;
  gap: 30px;
  align-items: center;
}

.nav-item {
  text-decoration: none;
  color: var(--text-color);
  font-size: 16px;
  font-weight: bold;
  transition: all 0.3s ease;
}

.nav-item:hover {
  text-decoration: underline;
}

.navbar-toggle {
  display: none;
  flex-direction: column;
  justify-content: space-between;
  width: 30px;
  height: 25px;
  cursor: pointer;
}

.bar {
  width: 100%;
  height: 5px;
  background-color: var(--text-color);
}

/* 页面内容样式 */
.content {
  padding: 20px;
  text-align: center;
}

/* 搜索框样式 */
.search-container {
  display: flex;
  align-items: center;
  background-color: #F5F2F2; /* 修改为 F5F2F2 背景色 */
  border-radius: 20px;
  border: 1px solid #ddd;
  padding: 5px;
  margin-left: 20px;
  width: 200px;
}

.search-btn {
  background-color: transparent;
  border: none;
  padding: 5px;
  cursor: pointer;
}

.search-btn i {
  font-size: 18px;
  color: var(--text-color);
}

.search-box {
  border: none;
  outline: none;
  padding: 8px;
  font-size: 14px;
  border-radius: 15px;
  width: 100%;
  background-color: #F5F2F2; 
}

/* 新增组块容器样式 */
.ingredients-container {
  display: flex;
  justify-content: space-between;
  margin-top: 30px;
  gap: 20px;
}

/* 每个部分样式 */
.ingredient-item {
  width: 23%; /* 四个部分宽度相等 */
  aspect-ratio: 5 / 13; /* 设置宽高比为 5:13 */
  border-radius: 30px;
  overflow: hidden;
  background-color: #F5F2F2;
  text-align: center;
  display: flex;
  flex-direction: column;
  justify-content: left;
  
}

.ingredient-image {
  width: 100%;
  height: 50%; /* 让图片占满整个容器 */
  object-fit: cover; /* 保持图片比例，裁剪图片以填充容器 */
  background-position: center;
}

.ingredient-text {
  font-size: 20px;
  font-weight: bold;
  padding: 10px 0;
  background-color: #F5F2F2;
  color: var(--text-color);
}

.dropdowns {
  display: flex;
  flex-wrap: wrap; /* 使下拉菜单在空间不足时换行 */
  justify-content: flex-start; /* 左对齐所有下拉菜单 */
  gap: 15px; /* 增加下拉菜单之间的间距 */
  padding: 10px; /* 添加内边距 */
  width: 100%; /* 确保容器宽度充满父元素 */
}

.dropdown {
  flex: 1 ; 
  min-width: 150px; /* 设置最小宽度 */
  max-width: 100%; /* 设置最大宽度 */
  height: 40px;
  border-radius: 40px; /* 圆角 */
  padding: 5px; /* 内边距 */
  border: 1px solid #ddd; /* 添加边框 */
  background-color: white; /* 背景色 */
  cursor: pointer;
  margin-right: 10px; /* 增加右边距，调整两个下拉菜单的左右间距 */
  transition: background-color 0.3s ease, border-color 0.3s ease;
}
/* 清除最后一个下拉菜单的右边距 */
.dropdown:last-child {
  margin-right: 0; /* 最后的下拉菜单不需要右边距 */
}
.dropdown:hover {
  background-color: #f5f5f5; /* 悬停时背景颜色变浅 */
  border-color: #ffdb63; /* 悬停时边框高亮 */
}

.dropdown + .dropdown {
  flex: 0.3; /* 右边的下拉菜单宽度比左边小 */

  min-width: 100px; /* 设置最小宽度，避免过于紧凑 */
  max-width: 100%; /* 设置最大宽度，避免过长 */
}

.extra-box {
  background-color: #F5F2F2; /* 背景色与 ingredient-item 相同 */
  border-radius: 30px; /* 圆角设置一致 */
  width: 100%; /* 默认填满容器宽度 */
  min-height: 250px; /* 初始高度，根据需要调整 */
  margin-top: 20px; /* 与上方的内容留出间距 */
  display: flex;
  justify-content: center; /* 居中内容 */
  align-items: center;
  flex-basis: 100%; /* 强制新盒子占据整个容器宽度 */
}


/* 响应式设计：移动端 */
@media (max-width: 768px) {
  .navbar {
    flex-direction: column;
    align-items: flex-start;
  }

  .navbar-right {
    display: flex;
    flex-direction: column;
    width: 100%;
    padding: 10px 0;
    display: none;
  }

  .navbar-right.open {
    display: flex;
  }

  .navbar-toggle {
    display: flex;
  }

  .search-container {
    width: 100%;
    margin-top: 10px;
  }
}

/* 新增图片和文字盒子 */
.image-box {
  width: 100%;
  height: 250px;
  background-image: url('./assets/istockphoto-1287579853-1024x1024.jpg'); /* 替换为你的图片路径 */
  background-size: cover;
  background-position: center;
  position: relative;
}

.overlay {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.5); /* 黑色不透明层 */
}

.text {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  text-align: center;
  color: white;
  font-weight: bold;
  font-family: 'Inter', sans-serif;
}

.text p {
  margin: 0;
  font-size: 30px;
  font-weight: bold;
}

.control-buttons {
  display: flex;
  justify-content: space-between;
  padding: 10px;
}

.control-buttons button {
  background-color: #ffdb63;
  border: none;
  padding: 5px 15px;
  font-size: 20px;
  border-radius: 15px;
  cursor: pointer;
}

.control-buttons button:hover {
  background-color: #f2c30f;
}

.match-buttons {
  display: flex;
  justify-content: center;
  gap: 50px; /* 增加下拉菜单之间的间距 */
  padding: 30px; /* 添加内边距 */
}

.match-buttons button {
  background-color: #ffdb63;
  border: none;
  padding: 5px 15px;
  font-size: 20px;
  border-radius: 15px;
  cursor: pointer;
}

.match-buttons button:hover {
  background-color: #f2c30f;
}

.footer {
  background-color: var(--theme-color); /* 黄色块背景 */
  color: black; /* 文本颜色为白色 */
  display: flex;
  justify-content: center;
  align-items: center;
  padding: 20px 40px; /* 内边距调整布局 */
  font-family: 'Inter', sans-serif;
}

.footer-left {
  display: flex;
  align-items: center;
}

.footer-logo {
  height: 150px; /* LOGO 高度 */
  width: auto; /* 保持比例 */
  padding: 15px; /* 添加内边距 */
}

.footer-right {
  display: flex;
  align-items: center;
  gap: 10px; /* 元素间距 */
}

.connect-text {
  font-size: 16px;
  font-weight: bold;
}

.social-icons a {
  color: black; /* 图标颜色为白色 */
  font-size: 35px; /* 图标大小 */
  text-decoration: none; /* 去掉下划线 */
  transition: color 0.3s ease;
  padding: 5px; /* 添加内边距 */
}

.social-icons a:hover {
  color: white; /* 悬停时变亮 */
}

</style>
