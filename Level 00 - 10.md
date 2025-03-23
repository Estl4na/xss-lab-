
Today, I spent time practicing and completing the XSS-Labs challenges. I went through all ten levels, learning how to identify and exploit different types of Cross-Site Scripting (XSS) vulnerabilities. Below is a detailed explanation of what I did in each level.

---

#### **Level 1: Basic Reflected XSS**
I started with Level 1, where the goal was to inject JavaScript code into the URL parameter `name` and trigger an alert box. I modified the URL like this:
![image](https://github.com/user-attachments/assets/a19d7798-0ca6-4b08-a6b5-6b9cb1519956)
```
?name=<script>alert(1)</script>
```
When I submitted the URL, the browser executed the script, and an alert box popped up with the number "1". This was my first step in understanding how reflected XSS works—user input is directly reflected on the page without proper sanitization.
![image](https://github.com/user-attachments/assets/58d25784-d266-4985-9d17-664c50f4b4c4)

---

#### **Level 2: Escaping Attributes**
![image](https://github.com/user-attachments/assets/8873deec-e6cc-46d1-9467-a7138e4d5d31)
In Level 2, I encountered a search form where my input was inserted into the `value` attribute of an `<input>` tag. Initially, injecting `<script>alert(1)</script>` didn’t work because the value was escaped. 
![image](https://github.com/user-attachments/assets/142e74a6-3aeb-43f4-be93-f35ce7ae79a5)

To bypass this, I closed the `value` attribute using `">` and injected my payload:
![image](https://github.com/user-attachments/assets/1f97ba4e-276c-4892-97fd-b1829aa88ef3)

```
"><script>alert(1)</script>
```
This worked because it broke out of the `value` attribute and allowed me to execute JavaScript. I learned that escaping attributes is crucial when exploiting XSS vulnerabilities.

![image](https://github.com/user-attachments/assets/d7c7d5fd-9261-4750-8440-d4fdaa18a933)
---

#### **Level 3: Event Injection**
![image](https://github.com/user-attachments/assets/11d2efb4-86bd-4c12-93b2-be25e6af2eda)

For Level 3, I noticed that the server filtered double quotes (`"`) and angle brackets (`< >`). 
![image](https://github.com/user-attachments/assets/cdc74523-2a32-4af0-ad3f-d31be3db7aa4)

Instead of trying to inject a full `<script>` tag, I used event handlers like `onclick` or `onfocus` to trigger JavaScript. My payload looked like this:
```
' onclick='alert(1)
```
I submitted the payload and clicked the input field, which triggered the alert box. This taught me that event-based injections can bypass filters that block `<script>` tags.
![image](https://github.com/user-attachments/assets/107ed7d0-4190-4eb0-9df6-0ddfba8ae870)

---

#### **Level 4: Bypassing Angle Bracket Filters**

In Level 4, the server removed angle brackets (`< >`) entirely. However, I realized that event handlers don’t always require these symbols. I crafted a payload using just double quotes to close the attribute and added an `onclick` event:
![image](https://github.com/user-attachments/assets/5cbe373d-5924-439b-9965-2091cbac99ab)
```
" onclick="alert(1)
```
After submitting the payload and interacting with the page, the alert box appeared. This reinforced the idea that even simple changes in payloads can bypass strict filters.

![image](https://github.com/user-attachments/assets/ebe30aa5-62c6-4cb9-b518-6456cc465739)

---

#### **Level 5: Pseudo-Protocol Exploitation**
![image](https://github.com/user-attachments/assets/23fa0cd0-3f8c-40f8-8cbd-59283467b956)

Level 5 introduced stricter filtering by breaking `<onclick>` tags with underscores (`<o_nclick>`). 
![image](https://github.com/user-attachments/assets/c5c32eb4-9d19-48cd-a9a1-4b01567d3639)

To bypass this, I avoided `<script>` altogether and used the `javascript:` pseudo-protocol inside an `<a>` tag:
```
"><a href="javascript:alert(1)">Click Me</a>
```
![image](https://github.com/user-attachments/assets/af92bef0-59a9-4ec5-896b-43a3c751c693)

When I clicked the link, the alert box popped up. This demonstrated how alternative methods like pseudo-protocols can be effective when traditional scripts are blocked.

---

#### **Level 6: Case Sensitivity Bypass**
![image](https://github.com/user-attachments/assets/a3259c4b-6eff-45f0-ad86-dd70e4fb5878)

In Level 6, both `<script>` and `<a>` tags were being altered. I noticed that HTML is case-insensitive, so I used mixed-case letters to bypass the filter:
```
"><a hrEF="javascript:alert(1)">
```
![image](https://github.com/user-attachments/assets/21e40556-25a4-4c72-8c19-36a0f7b98e14)

The payload worked perfectly, teaching me that manipulating letter cases can often circumvent naive filters.

---

#### **Level 7: Double Writing Strings**
![image](https://github.com/user-attachments/assets/f3063eac-ece7-4c7e-8288-1008a32b254e)

For Level 7, the server filtered the string `script`. To bypass this, I doubled the string within the payload:

```
"><scscriptript>alert(1)</scscriptript>
```

![image](https://github.com/user-attachments/assets/ee8d2eba-0443-4575-b874-f04db598e299)

This tricked the filter into removing only part of the string, leaving behind valid `<script>` tags. It was fascinating to see how creative string manipulation could defeat basic filtering mechanisms.

![image](https://github.com/user-attachments/assets/f216b001-9daa-4955-a0ee-a11d53186136)

---

#### **Level 8: Encoding Bypass**

![image](https://github.com/user-attachments/assets/da0dfc01-4f77-4402-81a8-09f1a660e196)

Level 8 required encoding the payload to bypass character restrictions. 

![image](https://github.com/user-attachments/assets/51d57d95-98a3-43f0-8210-3f882893cd2a)

The server encoded special characters like `<`, `>`, and `"`. Using an HTML encoder tool, I converted my payload into entities:

![image](https://github.com/user-attachments/assets/37c1429f-4f62-4caf-bf07-076e80703235)

```
&#106;&#97;&#118;&#97;&#115;&#99;&#114;&#105;&#112;&#116;&#58;&#97;&#108;&#101;&#114;&#116;&#40;&#49;&#41;
```
When I submitted this encoded payload, the browser decoded it and executed the JavaScript. This highlighted the importance of encoding techniques in evading filters.

---

#### **Level 9: Keyword Detection Bypass**

![image](https://github.com/user-attachments/assets/071aeb23-14af-4831-9a6e-ec71a759b080)

In Level 9, the server checked for valid URLs before accepting input. If the payload didn’t include `http://`, it would reject it. To bypass this, I appended `http://` to my payload:
```
java&#115;&#99;&#114;&#105;&#112;&#116;:alert('xsshttp://')
```
The payload passed validation and triggered the alert box when clicked. This exercise showed me how combining encoding with specific requirements (like including `http://`) can overcome additional checks.
![image](https://github.com/user-attachments/assets/026f80f6-ebd5-4689-8560-4e1d9f2cbc1b)

---

#### **Level 10: Hidden Field Injection**

![image](https://github.com/user-attachments/assets/bb16c81d-b11a-44a3-833b-d81929fedab2)

Finally, in Level 10, I discovered that certain hidden fields, like `t_sort`, were vulnerable. 

![image](https://github.com/user-attachments/assets/4dbda92d-5712-41db-88c7-d5639ad9d078)

By analyzing the source code, I identified that the `t_sort` field’s value was dynamically updated. I crafted a payload to inject malicious JavaScript:

```
"><input type="text" onclick="alert(1)">
```
![image](https://github.com/user-attachments/assets/2ceb7cff-bc53-4e68-8fa1-dae596b6c8a4)

Submitting this payload caused the alert box to appear when I interacted with the input field. This final challenge emphasized the need to thoroughly inspect all parts of the DOM, including hidden elements.

---

### **Summary**

Today, I spent a significant amount of time practicing XSS (Cross-Site Scripting) vulnerabilities through the XSS-Labs challenges. This experience was eye-opening and helped me understand not only how XSS works but also why it’s so critical to learn about these vulnerabilities as both a developer and a security professional.

#### **Why Learn XSS?**
XSS is one of the most common and dangerous web vulnerabilities because it allows attackers to inject malicious scripts into web pages viewed by other users. These scripts can steal sensitive information like cookies, session tokens, or even login credentials. Understanding XSS helps developers write secure code and protects users from potential attacks. Today’s practice made me realize that even small mistakes in input validation or output encoding can lead to significant security breaches.

#### **What Did I Learn Today?**
1. **Understanding Input Handling**: 
   - Every input field on a web page is a potential entry point for an attacker. By analyzing how data is processed, reflected, or stored, I learned to identify where vulnerabilities might exist.
   - For example, if user input is directly embedded into HTML without sanitization, it creates an opportunity for XSS.

2. **Different Types of XSS**:
   - **Reflected XSS**: Occurs when malicious input is immediately returned in the response (e.g., via URL parameters). It’s often used in phishing attacks.
   - **Stored XSS**: Happens when malicious input is saved on the server and later displayed to other users. This is more dangerous because it affects multiple victims.
   - **DOM-based XSS**: Involves JavaScript manipulating the DOM in ways that allow malicious scripts to execute. This type requires careful inspection of client-side code.

3. **Exploiting Vulnerabilities**:
   - I practiced injecting payloads into various fields—some were simple `<script>` tags, while others required creative techniques like event handlers (`onclick`, `onfocus`) or pseudo-protocols (`javascript:`).
   - Each level forced me to think critically about how the application processes inputs and outputs, teaching me to adapt my approach based on the specific filtering mechanisms in place.

4. **The Importance of Context**:
   - The same payload may work in one context but fail in another due to differences in how browsers render HTML or how servers sanitize inputs. For instance, some levels required escaping attributes with `">`, while others needed encoded payloads using HTML entities.

5. **Real-World Implications**:
   - Through tasks like session hijacking and phishing form injection, I saw firsthand how attackers could steal cookies or trick users into revealing their credentials. These exercises highlighted the importance of securing every layer of a web application.

#### **When to Use Which Method?**
- **Basic Payloads**: If no filters are in place, start with simple `<script>alert(1)</script>`.
- **Event Handlers**: When `<script>` tags are blocked, use events like `onclick` or `onfocus`. These are especially useful for forms or interactive elements.
- **Pseudo-Protocols**: If you’re dealing with links or buttons, try injecting `javascript:alert(1)` into attributes like `href`.
- **Encoding**: When special characters are filtered or encoded, convert your payload into HTML entities (e.g., `&#60;` for `<`).
- **Blind XSS**: For cases where you don’t see the output directly (e.g., admin panels), load remote scripts to confirm execution.

#### **Key Takeaways from Problem-Solving Approach**
1. **Analyzing Source Code**: Always check the raw source code to understand how inputs are handled. Tools like F12 Developer Tools are invaluable here.
2. **Testing Incrementally**: Start with basic payloads and gradually refine them based on error messages or behavior changes.
3. **Adapting to Filters**: Don’t give up if a payload fails—try alternative techniques like encoding, double-writing strings, or leveraging case sensitivity.
4. **Thinking Like an Attacker**: To defend against XSS, you need to think like an attacker. Assume every input is hostile until proven otherwise.

#### **Final Thoughts**
Today’s practice wasn’t just about solving puzzles—it was about developing a mindset focused on security. Every step reinforced the idea that prevention is better than cure. As a developer, I now appreciate the importance of properly sanitizing inputs, validating outputs, and implementing Content Security Policies (CSP). As a security enthusiast, I’m excited to continue exploring these concepts further.

![image](https://github.com/user-attachments/assets/0d9411b9-fbc1-41bf-8430-a26806674f75)
