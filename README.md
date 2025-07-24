````markdown
# 🕵️‍♂️ JavaScript Deobfuscation in Blogger Template

This project documents how I identified and removed a **malicious obfuscated redirect script** from a free Blogger theme (`LearnTechWithDeepak-Free-XML.xml`) that forcefully redirected users to `https://ltwdlearntechwithdeepak.blogspot.com/` when attribution was removed.

---

## 🧠 Problem Summary

- While customizing the Blogger theme, I noticed a **forced redirect** to a third-party site.
- Even after removing the footer credit (`#probtemplates`), the blog kept redirecting.
- After inspecting the XML, I discovered **obfuscated JavaScript** using `eval`, `_0x` hex variables, and escape sequences (`\x68`, `%6C` etc.).

---

## 🔍 Tools Used

### 🛠 JavaScript Deobfuscator:
[**de4js** – JavaScript Deobfuscator & Unpacker](https://lelinhtinh.github.io/de4js/)

---

## 📜 Steps to Decode Obfuscated JavaScript

### 1. **Copy Obfuscated Script**
From the `<script>` block in the theme XML (typically looks like):

```js
<script type='text/javascript'>
//<![CDATA[
eval(function(_0x...) { ... })
//]]>
</script>
````

---

### 2. **Go to De4js Tool**

Open: [https://lelinhtinh.github.io/de4js/](https://lelinhtinh.github.io/de4js/)

---

### 3. **Paste Script and Select Settings**

* Choose **Eval** decoding
* Enable:

  * ✅ Format Code
  * ✅ Unescape strings
  * ✅ Recover object path
  * ✅ Merge strings

Then click **"Auto Decode"**

---

### 4. **Analyze Decoded Code**

You'll typically find redirect logic like:

```js
setInterval(function () {
  if (!$('a#probtemplates').length || !$('a#probtemplates:visible').length) {
    window.location.href = 'https://ltwdlearntechwithdeepak.blogspot.com/';
  }
}, 1000);
```

This checks every second if the attribution link is removed — and redirects if it is.

---

### 5. **Fix: Remove or Replace the Code**

* Remove the entire `setInterval(...)` block
* Optionally replace the branding with your own:

```js
$('a#probtemplates').attr('href', 'https://yourdomain.com').text('Your Brand');
```

Wrap in CDATA if used in XML:

```xml
<script type='text/javascript'>
<![CDATA[
  // your clean JS here
]]>
</script>
```

---

## ✅ Final Result

* Attribution is now fully customizable
* Redirect removed
* Blog works perfectly with custom branding

---

## 📌 Credits

* Obfuscated Theme: `TechSpot-Free-XML.xml`
* Cleaned by: **Learn Tech With Deepak**
* Tools: [de4js](https://lelinhtinh.github.io/de4js/), Browser DevTools

---

## 💬 License & Use

Feel free to use this decoding workflow for any obfuscated Blogger theme or JS redirect removal project. Use responsibly and credit the original theme authors if you're just modifying, not building from scratch.

```

