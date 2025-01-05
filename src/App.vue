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
       <a href="#" class="nav-item" :class="{ active: currentPage === 'recipeSearch' }" @click="setActivePage('recipeSearch')">Recipe Search</a>
        <a href="#" class="nav-item" :class="{ active: currentPage === 'mealPlanner' }" @click="setActivePage('mealPlanner')">Meal Planner</a>
        <a href="#" class="nav-item" :class="{ active: currentPage === 'myPage' }" @click="setActivePage('myPage')">My Page</a>
        
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


    <!-- 页面主体内容 -->
    <main class="content">
    <!-- 根据当前页面条件渲染内容 -->
    <template v-if="currentPage === 'recipeSearch'">


    <!-- 新增的图片和文字盒子 -->
    <div class="image-box">
      <div class="overlay"></div>
      <div class="text">
        <p>Just tell us what ingredients do you have and</p>
        <p>we will show you</p>
        <p>what can you make.</p>
      </div>
    </div>

  <h1>Which ingredients are available?</h1>

  <!-- 新增的组块 -->
  <div class="ingredients-container">
  <!-- 动态渲染 ingredient-item -->
  <div class="ingredient-item" v-for="(ingredient, index) in ingredients" :key="index">
    <img class="ingredient-image" :src="ingredient.image" :alt="ingredient.name" />
    <div class="ingredient-text">{{ ingredient.name }}</div>
    <!-- 动态生成下拉菜单组 -->
    <div class="dropdowns">
      <div v-for="(dropdownGroup, idx) in ingredient.dropdowns" :key="idx" class="dropdown-group">
        <input class="dropdown" v-model="ingredient.dropdowns[idx].value" placeholder="Enter Ingredient" />        
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
      <button @click="fetchRecipes">Recipe Search</button>
      <button @click="fetchRandomRecipe">Random Recipe</button>
    </div>


  <!-- 新盒子 -->
    <!-- 食谱展示部分 -->
    <div class="extra-box" v-if="recipes.length > 0">
      <div v-for="(recipe, index) in recipes" 
      :key="index" 
      class="recipe" 
      :style="{ backgroundImage
      : 'url(' + recipe.image + ')' }" 
      @click="openRecipe(recipe.sourceUrl)">  
     
       <h3>{{ recipe.title }}</h3>
        <p><strong>Ready in:</strong> {{ recipe.readyInMinutes }} minutes</p> <!-- 显示 readyInMinutes -->
        <p><strong>Calories:</strong> {{ recipe.calories }} kcal</p>
        <p><strong>Protein:</strong> {{ recipe.readyInMinutes }} g</p>
        <p><strong>Carbohydrates:</strong> {{ recipe.readyInMinutes }} g</p>
        <p><strong>Fat:</strong> {{ recipe.readyInMinutes }} g</p>
        <p v-if="recipe.nutrition.length > 0">
          <strong>Nutrition:</strong>
          <ul>
            <li v-for="(nutrient, idx) in recipe.nutrition" :key="idx">
              {{ nutrient.title }}: {{ nutrient.amount }} {{ nutrient.unit }}
            </li>
          </ul>
        </p>
      </div>
    </div>


  <!-- Random食谱展示部分 -->
  <div class="random-recipe" v-if="randomRecipe && randomRecipe.title">
    <div class="random-recipe-item" 
       :key="randomRecipe.id" 
       :style="{ backgroundImage
       : 'url(' + randomRecipe.image + ')' }" 
       @click="openrandomRecipe(randomRecipe.strYoutube)">   
       <div class="recipe-overlay"></div>
 
       <h3> {{randomRecipe.title}} </h3>
       <p><strong>Area:</strong> {{ randomRecipe.Area }} </p>
       <p><strong>Ingredient:</strong> {{ randomRecipe.ingredient }} </p>
       <p><strong>Instructions:</strong> {{ randomRecipe.Instructions }} </p>
      </div>
      </div>

    </template>


  <template v-else-if="currentPage === 'underConstruction'">
    <!-- 正在开发页面 -->
    <UnderConstruction />
  </template>


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

import axios from 'axios';
import UnderConstruction from './components/UnderConstruction.vue';

export default {
      name: "App",


      components: {
    UnderConstruction,
  },



  data() {
    return {
        // 食材列表，每个食材项包含 name, image 和 dropdowns 数组
        ingredients: [
        {
          name: "Meat & Eggs",
          image: require("@/assets/meat-and-eggs.jpg"),
          dropdowns: [{value: "" }], // 初始有两组下拉菜单
        },
        {
          name: "Vegetables",
          image: require("@/assets/vegetables.jpg"),
          dropdowns: [{value: "" }], // 初始有两组下拉菜单
        },
        {
          name: "Staple Foods",
          image: require("@/assets/staple-foods.jpg"),
          dropdowns: [{value: "" }], // 初始有两组下拉菜单
        },
        {
          name: "Seasoning",
          image: require("@/assets/seasoning.jpg"),
          dropdowns: [{value: "" }], // 初始有两组下拉菜单
        },


      ],

      recipes: [],  // 用来存储返回的食谱
      allRecipes: [], // 存储所有食谱结果

      randomRecipe: [], 

      currentPage: 'recipeSearch', // 初始页面为 Recipe Search
      isNavOpen: false,

    };
  },
  methods: {



    toggleNav() {
      this.isNavOpen = !this.isNavOpen;
    },

    setActivePage(page) {
    if (page === 'mealPlanner' || page === 'myPage') {
      this.currentPage = 'underConstruction'; // 显示正在开发中的页面
    } else {
      this.currentPage = page; // 更新其他页面
    }
  },

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

    // 发送请求获取食谱数据
    async fetchRecipes() {

      // 清空之前的结果
      this.randomRecipe = [];
      this.recipes = [];
      this.allRecipes = [];

      const ingredientInputs = this.ingredients
        .map(ingredient => ingredient.dropdowns.map(dropdown => dropdown.value))// 获取每个食材的输入值
        .flat() // 将二维数组展平
        .filter(value => value); // 过滤空值

      if (ingredientInputs.length === 0) {
        return;
      }
      // 将输入的食材拼接成字符串
      const ingredientsQuery = ingredientInputs.join(","); // 例如 "apples,flour,sugar"

      // API 请求示例（使用实际的API URL和密钥替换）
      const apiUrl = `https://api.spoonacular.com/recipes/findByIngredients?ingredients=${ingredientsQuery}&number=2&apiKey=65e36080e70a424ca93ad4ad90390e51`;
      const apiKey = "65e36080e70a424ca93ad4ad90390e51";  //API密钥

      try {
        const response = await axios.get(apiUrl, {
          params: {
            ingredients: ingredientsQuery, // 将食材查询字符串传递到 API 请求中
            number: 2,  // 返回100个结果
            apiKey: apiKey,
          },
        });

        console.log("apiUrl:", apiUrl);

        
    // 提取食谱信息
      const allRecipes =  Array.isArray(response.data) ? response.data.map(recipe => ({
      id: recipe.id, // 直接获取 recipe 的 id
      title: recipe.title,
      image: recipe.image,
      readyInMinutes: "Loading...",
      nutrition: recipe.nutrition ? recipe.nutrition.nutrients : [],
      calories: "Loading...",
      Protein: "Loading...", 
      Carbohydrates: "Loading...", 
      Fat: "Loading...", 
      sourceUrl:[],
       })) : [];

        // 更新食谱数据
        this.allRecipes = allRecipes; // 存储所有数据
        this.updateRecipes(); // 更新当前显示数据
        // 获取每个食谱的详细信息
        this.recipes.forEach((recipe, index) => {
        this.fetchRecipeDetails(recipe.id, index);
      });


      } catch (error) {

        console.error("Error fetching recipes:", error);
      }


    },
     updateRecipes() {
     this.recipes = this.allRecipes;
    },

    async fetchRecipeDetails(recipeId, index) {
    const apiUrl = `https://api.spoonacular.com/recipes/${recipeId}/information?includeNutrition=true&`;
    const apiKey = "65e36080e70a424ca93ad4ad90390e51"; // 替换为实际密钥

    try {
      const response = await axios.get(apiUrl, {
        params: { apiKey: apiKey },
      });

      const calories = response.data.nutrition.nutrients.find(n => n.name === "Calories")?.amount || "N/A";
      const readyInMinutes = response.data.readyInMinutes || "N/A"; // 提取 readyInMinutes
      const Protein = response.data.nutrition.nutrients.find(n => n.name === "Protein")?.amount || "N/A";
      const Carbohydrates = response.data.nutrition.nutrients.find(n => n.name === "Carbohydrates")?.amount || "N/A";
      const Fat = response.data.nutrition.nutrients.find(n => n.name === "Fat")?.amount || "N/A";

      const sourceUrl = response.data.sourceUrl || "N/A";


      // 在 Vue 3 中直接赋值即可，不需要使用 $set
      this.recipes[index].calories = calories; // 动态更新对应食谱的 calories
      this.recipes[index].readyInMinutes = readyInMinutes; // 更新 readyInMinutes
      this.recipes[index].Protein = Protein; // 动态更新对应食谱的 calories
      this.recipes[index].Carbohydrates = Carbohydrates; // 动态更新对应食谱的 calories
      this.recipes[index].Fat = Fat; // 动态更新对应食谱的 calories
      this.recipes[index].sourceUrl = sourceUrl; // 动态更新对应食谱的 calories

      console.log("sourceUrl:", sourceUrl);


    } catch (error) {
      console.error(`Error fetching details for recipe ${recipeId}:`, error);
      this.recipes[index].calories = "Not found"; // 错误时也直接更新
      this.recipes[index].readyInMinutes = "Not found"; // 错误时更新 readyInMinutes
      this.recipes[index].Protein = "Not found"; // 错误时也直接更新
      this.recipes[index].Carbohydrates = "Not found"; // 错误时也直接更新
      this.recipes[index].Fat = "Not found"; // 错误时也直接更新

    }

    },

    openRecipe(url) {
    if (url && url !== "N/A") {
      window.open(url, "_blank"); // 打开链接到新标签页
    } else {
      alert("Recipe source URL not available.");
    }
    },




    // 发送请求获取random食谱数据
    async fetchRandomRecipe() {
      // 清空之前的结果
      this.randomRecipe = [];
      this.recipes = [];
      this.allRecipes = [];


      try {

  // 发起 API 请求
  const response = await axios.get('https://www.themealdb.com/api/json/v1/1/random.php');

// 检查返回的数据结构是否有效
if (!response.data || !response.data.meals || response.data.meals.length === 0) {
  console.error("Invalid API response:", response);
  return; // 终止后续操作
}


// 提取第一个食谱
const firstRecipe = response.data.meals[0];

// 获取所有原料和量度字段并将它们合并
const ingredients = [];
for (let i = 1; i <= 20; i++) {
  const ingredient = firstRecipe[`strIngredient${i}`];
  const measure = firstRecipe[`strMeasure${i}`];
  
  if (ingredient && measure) {
    // 将原料和量度拼接在一起，并加入到数组中
    ingredients.push(`${ingredient} (${measure})`);
  }
}

// 将所有拼接后的原料和量度用逗号分隔，组成一个字符串
const ingredientsString = ingredients.join(', ');

this.randomRecipe = {
  id: firstRecipe.idMeal,
  title: firstRecipe.strMeal,
  image: firstRecipe.strMealThumb,
  Area: firstRecipe.strArea,
  ingredient: ingredientsString, 
  Instructions:firstRecipe.strInstructions,
  strYoutube: firstRecipe.strYoutube,
};

// 打印日志
console.log("randomRecipe:", this.randomRecipe);

} catch (error) {
// 捕获并处理错误
console.error("Error fetching random recipe:", error);
}
},


openrandomRecipe(url) {
    if (url && url !== "N/A") {
      window.open(url, "_blank"); // 打开链接到新标签页
    } else {
      alert("Recipe source URL not available.");
    }
    },





  },

  watch: {
      currentPage(newPage) {
        let pageTitle = '';
        if (newPage === 'recipeSearch') {
          pageTitle = 'Recipe Search';
        } else if (newPage === 'mealPlanner') {
          pageTitle = 'Meal Planner';
        } else if (newPage === 'myPage') {
          pageTitle = 'My Page';
        }
        document.title = pageTitle; // 动态更新浏览器标题
      }
    }




};








</script>









<style>

.nav-item.active {
  color: black;  /* 激活时的颜色，可以根据需要调整 */
  font-weight: bold;
}


/* 定义主题变量 */
:root {
  --theme-color: #FFDB63;
  --text-color: gray;
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
  align-items: center; /* 垂直居中 */
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
  color: black;
}

.dropdowns {
  display: flex;
  flex-wrap: wrap; /* 使下拉菜单在空间不足时换行 */
  justify-content: center; /* 将内容水平居中 */
  align-items: center; /* 将内容垂直居中 */
  gap: 10px; /* 增加下拉菜单之间的间距 */
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
  transition: background-color 0.3s ease, border-color 0.3s ease;
  justify-content: space-between; /* 居中内容 */
  align-items: center; /* 垂直居中 */
}
/* 清除最后一个下拉菜单的右边距 */
.dropdown:last-child {
  margin-right: 0; /* 最后的下拉菜单不需要右边距 */
}
.dropdown:hover {
  background-color: #f5f5f5; /* 悬停时背景颜色变浅 */
  border-color: #ffdb63; /* 悬停时边框高亮 */
}

 
 
.extra-box {
  display: grid;
  grid-template-columns: repeat(1, 1fr); /* 每行 5 列 */
  gap: 20px; /* 添加间距 */
  background-color: #F5F2F2;
  border-radius: 30px;
  width: 100%;
  padding: 20px;
  margin-top: 20px;
}

.recipe {
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  background-size: cover;
  background-position: center;
  border-radius: 15px;
  align-items: center;
  height: 300px;
  color: white;
  padding: 15px;
  box-shadow: 0 4px 10px rgba(0, 0, 0, 0.3);
  position: relative;
}

/* 调整背景的对比度 */
.recipe {
  background-size: cover;
  background-position: center;
  cursor: pointer;
  transition: transform 0.2s ease-in-out;
}

/* 为文本添加一个半透明的黑色背景层 */
.recipe h3, .recipe p {
  position: relative;
  z-index: 1;
  background-color: #ffdb63(0, 0, 0, 0.5); /* 半透明黑色背景 */
  border-radius: 8px;
  padding: 5px 10px;
  margin: 5px 0;
}


.recipe h3 {
  font-size: 20px;
  margin: 0;
  line-height: 1.2;
  color: white;
  text-shadow: 2px 2px 5px rgba(0, 0, 0, 0.7); /* 给标题添加阴影 */
}

.recipe p {
  font-size: 14px; /* 调整字体大小 */
  line-height: 1; /* 增加行间距 */
  margin-bottom: 3px; /* 增加段落之间的间距 */
  text-shadow: 1px 1px 4px rgba(0, 0, 0, 0.7); /* 给段落添加阴影 */
}
.recipe p strong {
  font-weight: bold; /* 加粗内容，增强视觉效果 */
}

.recipe ul {
  list-style-type: none;
  padding: 0;
  margin: 0;
}

.recipe ul li {
  font-size: 12px;
  line-height: 0.5; /* 增加行间距 */
  margin-bottom: 5px; /* 调整列表项的间距 */
}


/* 增加食谱展示卡片的内边距，确保内容不会太拥挤 */
.recipe {
  padding: 5px;
}
.recipe:hover {
  transform: scale(1.05);
}

/* 提高内容块的视觉分隔度 */
.recipe p + p {
  margin-top: 5px; /* 增加相邻段落的间距 */
}

.recipe-overlay {
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background: var(--theme-color)rgba(0, 0, 0, 0.4); /* 半透明遮罩层 */
}

.random-recipe h3, .random-recipe p {
  z-index: 1; /* 确保文字显示在遮罩层上方 */
  background: var(--theme-color)rgba(0, 0, 0, 0.4); /* 半透明遮罩层 */
}


.random-recipe {
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  background-size: cover;
  background-position: center;
  border-radius: 30px;
  align-items: center;
  height: 300px;
  color: #F5F2F2;
  padding: 15px;
  box-shadow: 0 4px 10px rgba(0, 0, 0, 0.3);
  position: relative;
/*   background-size: cover;
  background-position: center;
 */  cursor: pointer;
  transition: transform 0.2s ease-in-out;
  display: grid;
  margin-top: 1px;
  margin-bottom: 1px;

}
.random-recipe:hover {
  transform: scale(1.05);
}


.random-recipe h3 {
  font-size: 20px;
  margin: 0;
  line-height: 1;
  color: white;
  text-shadow: 2px 2px 5px rgba(0, 0, 0, 0.7); /* 给标题添加阴影 */
}

.random-recipe p {
  font-size: 14px; /* 调整字体大小 */
  line-height: 1.5; /* 增加行间距 */
  margin-bottom: 3px; /* 增加段落之间的间距 */
  text-shadow: 1px 1px 4px rgba(0, 0, 0, 0.7); /* 给段落添加阴影 */
  color: white;
  text-align: left;

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

  .recipe h3 {
    font-size: 18px;
  }

  .recipe p {
    font-size: 14px;
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
