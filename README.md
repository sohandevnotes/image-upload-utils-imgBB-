# 🖼️ ImgBB Image Upload Utility – Setup Guide

This guide explains how to create a reusable `imageupload` utility using ImgBB, store your API key securely using `.env.local`, and use the function anywhere in your project.

---

# 📁 1. Create `utils` Folder

Inside the **`src`** directory of your project, create a folder named:

```
src/utils
```

---

# 📄 2. Create `index.js` Inside `utils`

Inside the newly created folder, create a file:

```
src/utils/index.js
```

---

# 📦 3. Add Your Image Upload Utility Code

Open `index.js` and paste the following code:

```js
import axios from "axios";

export const imageupload = async (imageData) => {
  const formData = new FormData();
  formData.append("image", imageData);

  const { data } = await axios.post(
    `https://api.imgbb.com/1/upload?&key=${import.meta.env.VITE_IMGBB_API_KEY}`,
    formData
  );

  return data?.data?.display_url;
};
```

---

# 🔍 How This Works

### ✔ Step-by-step Explanation:

### **1. axios import**

We use **axios** to make HTTP POST requests:

```js
import axios from "axios";
```

### **2. Create a FormData object**

ImgBB requires uploaded images to be sent as form-data:

```js
const formData = new FormData();
formData.append("image", imageData);
```

Your `imageData` should be:

* a `File` object (from `<input type="file" />`)
* or a blob

### **3. Call ImgBB API**

The request is sent to:

```
https://api.imgbb.com/1/upload
```

with your secret API key, stored securely in **`.env.local`**:

```js
import.meta.env.VITE_IMGBB_API_KEY
```

### **4. Return the image URL**

After successful upload, ImgBB returns a URL:

```js
return data?.data?.display_url;
```

You can now save or display this public image link anywhere.

---

# 🔐 4. Create `.env.local` for ImgBB API Key

Inside your project root, create:

```
.env.local
```

Add your ImgBB API key:

```env
VITE_IMGBB_API_KEY=YOUR_IMGBB_API_KEY_HERE
```

---

# 🚫 5. Add to `.gitignore`

To keep your key safe:

```
.env
.env.local
```

---

# 🧪 6. How to Use `imageupload` in Your Components

Example usage inside a React component:

```jsx
import { imageupload } from "../utils";

const handleImageChange = async (e) => {
  const file = e.target.files[0];
  const url = await imageupload(file);
  console.log("Uploaded Image URL:", url);
};
```

Example file input:

```jsx
<input type="file" onChange={handleImageChange} />
```

---

# 🎉 Summary

You now have:

✔ A reusable **Image upload utility**
✔ Secure `.env.local` variable for ImgBB API key
✔ Clean axios integration with FormData
✔ Easy way to upload images and get public URLs
