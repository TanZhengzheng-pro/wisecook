# Vue.js ウェブサイトの技術的要点

このウェブサイトは、以下の技術を使用しています。

## Vue.js の使用

- **Vue.js** を使用して、ウェブサイトのコンポーネント、データバインディング、条件付きレンダリング（v-if、v-else-if）を利用し、動的にページを更新しています。
- 食材データ（例：`ingredients`）を `data` で定義し、`v-for` を使って食材オプションを動的に生成しています。
- `v-model` によってユーザーが入力した食材を双方向にバインディングしています。
- **axios** を用いて非同期リクエストを行い、レシピ情報を取得し、コンポーネントデータを更新します。

## API リクエスト

- **Spoonacular** と **MealDB** の2つの外部APIを使用して、レシピの検索とランダムレシピ機能を実装しています。
- `fetchRecipes` 関数は、提供された食材を基にレシピを取得し、`recipes` と `allRecipes` 配列を更新します。
- `fetchRandomRecipe` 関数は、MealDB API を使ってランダムなレシピを取得します。
- **axios** を使用して GET リクエストを送信し、APIレスポンスを処理します。特に、異なるデータ（レシピの詳細など）の処理に注意しています。

## 動的データ更新

- `fetchRecipeDetails` 関数は、各レシピの詳細情報（例：カロリー、栄養成分）を取得し、`recipes` 配列内の該当フィールドを更新します。
- `v-for` と `v-bind` を使って、レシピ情報（画像、タイトル、栄養成分など）を動的にレンダリングしています。

## レスポンシブデザイン

- 検索ボックス、食材リスト、レシピ表示部分など、ページ内の一部のエリアは CSS スタイルでレスポンシブデザインを実現し、異なる画面サイズで適切に表示されます。
- **flexbox** と **grid** を使ったレイアウトで、要素が自動的に調整されます。
- **navbar-toggle** クラスは、モバイル端末でナビゲーションバーを切り替えるために使用され、シンプルなメニューが表示されます。

## ユーザーインタラクション

- ユーザーはボタンをクリックすることで、表示されるページを更新できます（例：「Meal Planner」や「Recipe Search」への切り替え）。
- `addDropdown` と `removeDropdown` メソッドにより、ユーザーは食材の入力フォーム（ドロップダウンメニュー）を動的に追加または削除できます。
- レシピ表示部分では、詳細情報を確認するためにクリックできるリンクがあり、外部リンクを開くボタンも提供されています。


# Recipe Finder - Vue.js Application

This project is built with Vue.js and utilizes various features like components, data binding, and conditional rendering for dynamic page updates. Below is an overview of key technical details and features.

## Key Features

### 1. **Vue.js Application**
- **Dynamic Page Updates**: Utilizes Vue components, data binding, and conditional rendering (`v-if`, `v-else-if`) to update the page in real-time.
- **Data Binding**: Ingredient data is defined in the `data` object (e.g., `ingredients`), and `v-for` is used to dynamically generate ingredient options.
- **Two-Way Binding**: `v-model` is used to bind user input (ingredients) dynamically.
- **Axios for Asynchronous Requests**: Axios is used for API requests to fetch recipe data and update the component's data.

### 2. **API Integration**
- **Spoonacular & MealDB APIs**: External APIs are used to fetch recipes. Spoonacular API is used for ingredient-based search, while MealDB is used for fetching random recipes.
- **Fetching Recipes**: The `fetchRecipes` function sends a request to fetch recipes based on provided ingredients, updating the `recipes` and `allRecipes` arrays.
- **Fetching Random Recipes**: The `fetchRandomRecipe` function fetches random recipes from MealDB.
- **Handling API Responses**: Axios sends GET requests and handles different response data (e.g., recipe details like ingredients, nutrition).

### 3. **Dynamic Data Updates**
- **Recipe Details**: The `fetchRecipeDetails` function retrieves detailed information (e.g., calories, nutrition) for each recipe and updates the respective fields in the `recipes` array.
- **Dynamic Rendering**: `v-for` and `v-bind` are used to dynamically display recipe details, such as images, titles, and nutrition information.

### 4. **Responsive Design**
- **Responsive Layout**: CSS styles ensure the app has a responsive layout, providing a great user experience on different screen sizes.
- **Flexbox & Grid**: These techniques are used to create layouts that adjust dynamically to screen size.
- **Mobile Navigation**: The `navbar-toggle` class allows the navigation bar to be toggled on mobile devices, ensuring a clean and functional mobile menu.

### 5. **User Interactions**
- **Interactive Buttons**: Users can click buttons to navigate between pages (e.g., "Meal Planner" and "Recipe Search").
- **Dynamic Dropdowns**: The `addDropdown` and `removeDropdown` methods allow users to add or remove ingredient input fields (dropdowns).
- **Recipe Details**: Users can click on a recipe to view detailed information, with a button to open external recipe links.

## Conclusion

This Vue.js app effectively combines responsive design, real-time updates, and dynamic user interactions to provide a seamless and interactive recipe search and meal planning experience.



# 项目技术说明

本项目基于 Vue.js 框架开发，主要实现了通过输入食材来搜索和显示食谱。以下是技术要点的详细描述。

## 技术栈

- **Vue.js**: 用于构建用户界面，通过组件、数据绑定和条件渲染动态更新页面。
- **Axios**: 用于发送异步 HTTP 请求，获取食谱数据。
- **Spoonacular API**: 提供食谱搜索功能。
- **MealDB API**: 提供随机食谱功能。

## 技术要点

### Vue.js 相关

- **组件和数据绑定**：应用通过 Vue.js 的组件机制和数据绑定功能来实现动态页面更新。Vue 的 `v-if` 和 `v-else-if` 用于条件渲染，确保页面根据用户的交互动态变化。
- **数据定义与动态渲染**：使用 `data` 定义食材数据（如 `ingredients`），并通过 `v-for` 指令动态生成食材输入框。用户可以通过输入框选择食材。
- **双向绑定**：通过 `v-model` 实现食材输入的双向绑定，方便获取和更新用户的输入。
- **异步请求**：使用 `axios` 发起 API 请求，获取食谱信息，并通过 Vue 的方法更新组件的数据。

### API 请求

- **食谱搜索**：通过调用外部 API（Spoonacular 和 MealDB）来获取食谱数据。`fetchRecipes` 函数会根据用户输入的食材发送请求，获取相关食谱并更新 `recipes` 和 `allRecipes` 数组。
- **随机食谱**：`fetchRandomRecipe` 函数通过 MealDB API 获取随机食谱。
- **处理 API 响应**：在 API 请求成功后，通过 `axios` 的响应来处理数据并更新 Vue 组件中的数据，特别是处理食谱的细节信息（如热量、营养成分等）。

### 动态数据更新

- **获取食谱详细信息**：`fetchRecipeDetails` 函数用于获取每个食谱的详细信息，包括热量、营养成分等，并将这些信息更新到相应的 `recipes` 数组中的字段。
- **动态渲染**：通过 `v-for` 和 `v-bind` 实现食谱数据的动态渲染。食谱的图片、标题和营养成分等信息会根据 API 响应自动展示在页面上。

### 响应式设计

- **自适应布局**：页面设计采用响应式设计，使用 CSS 样式确保在不同屏幕尺寸下都有良好的显示效果。
- **Flexbox 和 Grid 布局**：使用 `flexbox` 和 `grid` 布局方式，让页面元素能够根据屏幕大小自适应调整。
- **移动端导航栏**：`navbar-toggle` 类用于在移动设备上切换导航栏，提供简洁的菜单，使得用户在小屏幕设备上依然可以方便地进行操作。

### 用户交互

- **页面切换**：用户可以通过点击按钮切换不同页面（如切换到“Meal Planner”或“Recipe Search”）。
- **动态添加/删除食材**：`addDropdown` 和 `removeDropdown` 方法允许用户动态添加或删除食材输入框（下拉菜单），增强用户交互体验。
- **食谱详情展示**：食谱展示部分支持点击查看详细信息，并且提供按钮让用户打开食谱的外部链接，获取更多相关信息。

## 总结

该应用通过 Vue.js 强大的组件化机制和响应式设计，使得用户可以便捷地根据食材搜索食谱，并查看详细信息。同时，应用采用了最新的前端技术，如 Flexbox 和 Grid 布局来确保响应式设计，提升用户体验。
