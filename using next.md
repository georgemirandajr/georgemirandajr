# Using Next.js

Here are some key points for me to remember:
1. Use tailwind 
```
npm install --save-dev tailwindcss postcss-preset-env
npx tailwind init

// create postcss.config.js at root 
module.exports = {
  plugins: ['tailwindcss', 'postcss-preset-env'],
}

// index.css to styles dir
@tailwind base;
@tailwind components;
@tailwind utilities;

// import to _app.js
import '../styles/index.css';
```
2. Use `useContext` to manage state!
3. Use Auth0 for **easy** authentication. 
4. Use `useState` and combine data into one object when submitting forms.
