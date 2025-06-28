# HTML Notes

## Table of Contents
1. [Introduction](#introduction)
2. [HTML Boilerplate](#html-boilerplate)
3. [Document Structure](#document-structure)
4. [Text Content & Formatting](#text-content--formatting)
5. [Lists](#lists)
6. [Media Elements](#media-elements)
7. [Tables](#tables)
8. [Links & Navigation](#links--navigation)
9. [Forms](#forms)
10. [Semantic HTML](#semantic-html)
11. [Characters & Symbols](#characters--symbols)
12. [Meta Tags](#meta-tags)
13. [CSS & JavaScript Integration](#css--javascript-integration)
14. [Comments](#comments)
15. [Emmet Shortcuts](#emmet-shortcuts)
16. [Best Practices](#best-practices)

---

## Introduction

**HTML (HyperText Markup Language)** is the standard markup language for creating web pages. It describes the structure of a web page using markup elements (tags) that tell the browser how to display content.

### Key Features:
- **Structure**: Defines the skeleton of web pages
- **Semantic**: Provides meaning to content
- **Universal**: Supported by all web browsers
- **Foundation**: Base for all web technologies

---

## HTML Boilerplate

The basic HTML5 document structure:

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document Title</title>
</head>
<body>
    <!-- Page content goes here -->
</body>
</html>
```

### Explanation:
- `<!DOCTYPE html>`: Declares HTML5 document type
- `<html lang="en">`: Root element with language attribute
- `<head>`: Contains metadata (not visible on page)
- `<meta charset="UTF-8">`: Character encoding
- `<meta name="viewport">`: Responsive design viewport
- `<title>`: Page title (appears in browser tab)
- `<body>`: Contains visible page content

---

## Document Structure

### Container Elements

Container tags group and organize content:

#### div (Division)
```html
<div class="content-section">
    <p>This paragraph is inside the div container</p>
    <span>This span is also inside the div</span>
</div>
```

#### span (Inline Container)
```html
<p>This is a paragraph with a <span class="highlight">highlighted word</span> inside.</p>
```

### Headings

HTML provides six levels of headings for hierarchical content structure:

```html
<!-- Main heading, typically the page title -->
<h1>Main Page Title</h1>

<!-- Section headings -->
<h2>Section Heading</h2>

<!-- Subsection headings -->
<h3>Subsection Heading</h3>

<!-- Minor section headings -->
<h4>Minor Section Heading</h4>

<!-- Small headings -->
<h5>Small Heading</h5>

<!-- Smallest heading -->
<h6>Smallest Heading</h6>
```

**Best Practices:**
- Use only one `<h1>` per page
- Don't skip heading levels (h1 → h3)
- Use headings for structure, not styling

---

## Text Content & Formatting

### Paragraph and Text Elements
#### Paragraph
```html
<p>This is a paragraph of text.</p>
```

#### Preformatted Text
```html
<pre>
This text preserves    spaces
    and line breaks
        exactly as written.
</pre>
```

#### Code
```html
<!-- Inline code -->
<p>Use the <code>console.log()</code> function to debug.</p>

<!-- Code block -->
<pre><code>
function greet(name) {
    return `Hello, ${name}!`;
}
</code></pre>
```

### Text Formatting Tags

```html
<!-- Strong importance (bold) -->
<p>This is <strong>very important</strong> text.</p>

<!-- Emphasis (italic) -->
<p>This is <em>emphasized</em> text.</p>

<!-- Bold appearance (no semantic meaning) -->
<p>This is <b>bold</b> text.</p>

<!-- Italic appearance (no semantic meaning) -->
<p>This is <i>italic</i> text.</p>

<!-- Underlined text -->
<p>This is <u>underlined</u> text.</p>

<!-- Strikethrough text -->
<p>This is <s>strikethrough</s> text.</p>

<!-- Marked/highlighted text -->
<p>This is <mark>highlighted</mark> text.</p>

<!-- Small text -->
<p>This is <small>small</small> text.</p>

<!-- Subscript -->
<p>Chemical formula: H<sub>2</sub>O</p>

<!-- Superscript -->
<p>Mathematical expression: E = mc<sup>2</sup></p>

<!-- Line break -->
<p>First line<br>Second line</p>

<!-- Horizontal rule -->
<hr>
```

### Quotations
```html
<!-- Block quote -->
<blockquote cite="https://example.com">
    <p>This is a long quotation that spans multiple lines.</p>
    <footer>— Author Name</footer>
</blockquote>

<!-- Inline quote -->
<p>He said, <q>This is a short quote.</q></p>

<!-- Citation -->
<p>Reference: <cite>The Great Gatsby</cite></p>
```

---

## Lists
HTML supports different types of lists for organizing content:

### Ordered Lists (ol)
```html
<!-- Default numbered list -->
<ol>
    <li>First item</li>
    <li>Second item</li>
    <li>Third item</li>
</ol>

<!-- Custom numbering -->
<ol type="A" start="3">
    <li>Item C</li>
    <li>Item D</li>
    <li>Item E</li>
</ol>

<!-- Roman numerals -->
<ol type="I">
    <li>Introduction</li>
    <li>Main Content</li>
    <li>Conclusion</li>
</ol>
```

### Unordered Lists (ul)
```html
<!-- Default bullet points -->
<ul>
    <li>First item</li>
    <li>Second item</li>
    <li>Third item</li>
</ul>

<!-- Nested lists -->
<ul>
    <li>Main item 1
        <ul>
            <li>Sub-item 1.1</li>
            <li>Sub-item 1.2</li>
        </ul>
    </li>
    <li>Main item 2</li>
</ul>
```

### Description Lists (dl)
```html
<dl>
    <dt>HTML</dt>
    <dd>HyperText Markup Language</dd>
    
    <dt>CSS</dt>
    <dd>Cascading Style Sheets</dd>
    
    <dt>JavaScript</dt>
    <dd>Programming language for web interactivity</dd>
</dl>
```

---

## Media Elements
HTML provides various elements for embedding media content:

### Images
```html
<!-- Basic image -->
<img src="path/to/image.jpg" alt="Description of image">

<!-- Responsive image with multiple sources -->
<picture>
    <source media="(min-width: 800px)" srcset="large-image.jpg">
    <source media="(min-width: 400px)" srcset="medium-image.jpg">
    <img src="small-image.jpg" alt="Responsive image">
</picture>

<!-- Image with additional attributes -->
<img src="photo.jpg" 
     alt="Beautiful sunset" 
     width="300" 
     height="200" 
     loading="lazy"
     title="Sunset at the beach">
```

### Audio
```html
<!-- Basic audio with controls -->
<audio controls>
    <source src="audio.mp3" type="audio/mpeg">
    <source src="audio.ogg" type="audio/ogg">
    Your browser does not support the audio element.
</audio>

<!-- Audio with additional attributes -->
<audio controls autoplay muted loop>
    <source src="background-music.mp3" type="audio/mpeg">
</audio>
```

### Video
```html
<!-- Basic video -->
<video width="640" height="360" controls>
    <source src="movie.mp4" type="video/mp4">
    <source src="movie.webm" type="video/webm">
    Your browser does not support the video tag.
</video>

<!-- Video with poster and attributes -->
<video width="640" height="360" controls poster="thumbnail.jpg" preload="metadata">
    <source src="movie.mp4" type="video/mp4">
    <track kind="subtitles" src="subtitles.vtt" srclang="en" label="English">
</video>
```

### Figure and Caption
```html
<figure>
    <img src="chart.png" alt="Sales data chart">
    <figcaption>
        <strong>Figure 1:</strong> Monthly sales data for 2024
    </figcaption>
</figure>

<figure>
    <video controls width="400">
        <source src="tutorial.mp4" type="video/mp4">
    </video>
    <figcaption>Video tutorial: Getting started with HTML</figcaption>
</figure>
```

### Embedded Content
```html
<!-- YouTube video embed -->
<iframe width="560" height="315" 
        src="https://www.youtube.com/embed/VIDEO_ID" 
        frameborder="0" 
        allowfullscreen>
</iframe>

<!-- Google Maps embed -->
<iframe src="https://www.google.com/maps/embed?pb=..." 
        width="600" 
        height="450" 
        allowfullscreen="" 
        loading="lazy">
</iframe>
```

---

## Tables
Tables organize data in rows and columns:

### Basic Table Structure
```html
<table>
    <caption>Employee Information</caption>
    
    <thead>
        <tr>
            <th>Name</th>
            <th>Position</th>
            <th>Salary</th>
        </tr>
    </thead>
    
    <tbody>
        <tr>
            <td>John Doe</td>
            <td>Developer</td>
            <td>$70,000</td>
        </tr>
        <tr>
            <td>Jane Smith</td>
            <td>Designer</td>
            <td>$65,000</td>
        </tr>
    </tbody>
    
    <tfoot>
        <tr>
            <td colspan="2">Total</td>
            <td>$135,000</td>
        </tr>
    </tfoot>
</table>
```

### Advanced Table Features
```html
<table border="1" cellpadding="10" cellspacing="0">
    <caption>Product Comparison</caption>
    
    <thead>
        <tr>
            <th rowspan="2">Product</th>
            <th colspan="2">Specifications</th>
            <th rowspan="2">Price</th>
        </tr>
        <tr>
            <th>CPU</th>
            <th>RAM</th>
        </tr>
    </thead>
    
    <tbody>
        <tr>
            <td>Laptop A</td>
            <td>Intel i5</td>
            <td>8GB</td>
            <td>$799</td>
        </tr>
        <tr>
            <td>Laptop B</td>
            <td>Intel i7</td>
            <td>16GB</td>
            <td>$1,299</td>
        </tr>
    </tbody>
</table>
```

### Table Accessibility
```html
<table>
    <caption>Monthly Sales Report</caption>
    
    <thead>
        <tr>
            <th scope="col" id="month">Month</th>
            <th scope="col" id="sales">Sales</th>
            <th scope="col" id="growth">Growth</th>
        </tr>
    </thead>
    
    <tbody>
        <tr>
            <th scope="row" headers="month">January</th>
            <td headers="sales">$50,000</td>
            <td headers="growth">+5%</td>
        </tr>
    </tbody>
</table>
```

---

## Links & Navigation
The anchor tag creates hyperlinks for navigation:

### Basic Links
```html
<!-- External link -->
<a href="https://www.example.com">Visit Example.com</a>

<!-- Internal link -->
<a href="about.html">About Us</a>

<!-- Email link -->
<a href="mailto:contact@example.com">Send Email</a>

<!-- Phone link -->
<a href="tel:+1234567890">Call Us</a>

<!-- Link to section on same page -->
<a href="#section1">Go to Section 1</a>
```

### Link Attributes
```html
<!-- Open in new tab -->
<a href="https://example.com" target="_blank" rel="noopener noreferrer">
    External Link
</a>

<!-- Download link -->
<a href="document.pdf" download="my-document.pdf">Download PDF</a>

<!-- Link with title (tooltip) -->
<a href="https://example.com" title="Visit our homepage">Example</a>

<!-- Link with access key -->
<a href="contact.html" accesskey="c">Contact (Alt+C)</a>
```

### Navigation Menus
```html
<nav>
    <ul>
        <li><a href="#home">Home</a></li>
        <li><a href="#about">About</a></li>
        <li><a href="#services">Services</a></li>
        <li><a href="#contact">Contact</a></li>
    </ul>
</nav>

<!-- Breadcrumb navigation -->
<nav aria-label="Breadcrumb">
    <ol>
        <li><a href="/">Home</a></li>
        <li><a href="/products">Products</a></li>
        <li><a href="/products/laptops">Laptops</a></li>
        <li aria-current="page">Gaming Laptops</li>
    </ol>
</nav>
```

---

## Forms
Forms collect user input and send data to servers:

### Basic Form Structure
```html
<form action="/submit" method="POST" enctype="multipart/form-data">
    <!-- Form controls go here -->
    <button type="submit">Submit</button>
</form>
```

### Input Types
```html
<form>
    <!-- Text inputs -->
    <label for="username">Username:</label>
    <input type="text" id="username" name="username" required>
    
    <label for="email">Email:</label>
    <input type="email" id="email" name="email" required>
    
    <label for="password">Password:</label>
    <input type="password" id="password" name="password" minlength="8" required>
    
    <label for="phone">Phone:</label>
    <input type="tel" id="phone" name="phone" pattern="[0-9]{3}-[0-9]{3}-[0-9]{4}">
    
    <label for="website">Website:</label>
    <input type="url" id="website" name="website">
    
    <!-- Number inputs -->
    <label for="age">Age:</label>
    <input type="number" id="age" name="age" min="18" max="100">
    
    <label for="price">Price:</label>
    <input type="range" id="price" name="price" min="0" max="1000" step="10">
    
    <!-- Date and time -->
    <label for="birthday">Birthday:</label>
    <input type="date" id="birthday" name="birthday">
    
    <label for="appointment">Appointment:</label>
    <input type="datetime-local" id="appointment" name="appointment">
    
    <label for="meeting-time">Meeting Time:</label>
    <input type="time" id="meeting-time" name="meeting-time">
    
    <!-- File upload -->
    <label for="resume">Resume:</label>
    <input type="file" id="resume" name="resume" accept=".pdf,.doc,.docx">
    
    <!-- Hidden input -->
    <input type="hidden" name="form-id" value="contact-form">
</form>
```

### Form Controls
```html
<form>
    <!-- Textarea -->
    <label for="message">Message:</label>
    <textarea id="message" name="message" rows="4" cols="50" 
              placeholder="Enter your message here..."></textarea>
    
    <!-- Select dropdown -->
    <label for="country">Country:</label>
    <select id="country" name="country" required>
        <option value="">Select a country</option>
        <option value="us">United States</option>
        <option value="ca">Canada</option>
        <option value="uk">United Kingdom</option>
    </select>
    
    <!-- Multiple select -->
    <label for="skills">Skills:</label>
    <select id="skills" name="skills" multiple>
        <option value="html">HTML</option>
        <option value="css">CSS</option>
        <option value="js">JavaScript</option>
        <option value="python">Python</option>
    </select>
    
    <!-- Radio buttons -->
    <fieldset>
        <legend>Gender:</legend>
        <input type="radio" id="male" name="gender" value="male">
        <label for="male">Male</label>
        
        <input type="radio" id="female" name="gender" value="female">
        <label for="female">Female</label>
        
        <input type="radio" id="other" name="gender" value="other">
        <label for="other">Other</label>
    </fieldset>
    
    <!-- Checkboxes -->
    <fieldset>
        <legend>Interests:</legend>
        <input type="checkbox" id="sports" name="interests" value="sports">
        <label for="sports">Sports</label>
        
        <input type="checkbox" id="music" name="interests" value="music">
        <label for="music">Music</label>
        
        <input type="checkbox" id="travel" name="interests" value="travel">
        <label for="travel">Travel</label>
    </fieldset>
    
    <!-- Buttons -->
    <button type="submit">Submit Form</button>
    <button type="reset">Reset Form</button>
    <button type="button" onclick="alert('Button clicked!')">Custom Action</button>
</form>
```

### Advanced Form Features
```html
<form>
    <!-- Datalist for autocomplete -->
    <label for="browser">Choose a browser:</label>
    <input list="browsers" id="browser" name="browser">
    <datalist id="browsers">
        <option value="Chrome">
        <option value="Firefox">
        <option value="Safari">
        <option value="Edge">
    </datalist>
    
    <!-- Progress bar -->
    <label for="progress">Upload Progress:</label>
    <progress id="progress" value="60" max="100">60%</progress>
    
    <!-- Meter -->
    <label for="disk-usage">Disk Usage:</label>
    <meter id="disk-usage" value="0.6" min="0" max="1">60%</meter>
    
    <!-- Output element -->
    <form oninput="result.value=parseInt(a.value)+parseInt(b.value)">
        <input type="range" id="a" value="50">
        +<input type="number" id="b" value="25">
        =<output name="result" for="a b">75</output>
    </form>
</form>
```

### Form Validation
```html
<form>
    <!-- Required field -->
    <input type="email" required>
    
    <!-- Pattern validation -->
    <input type="text" pattern="[A-Za-z]{3,}" title="Minimum 3 letters">
    
    <!-- Length validation -->
    <input type="password" minlength="8" maxlength="20">
    
    <!-- Number range -->
    <input type="number" min="1" max="100">
    
    <!-- Custom validation message -->
    <input type="email" required 
           oninvalid="this.setCustomValidity('Please enter a valid email')"
           oninput="this.setCustomValidity('')">
</form>
```

---

## Semantic HTML

Semantic HTML provides meaning to web content, improving accessibility and SEO:

### Document Structure
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Semantic HTML Example</title>
</head>
<body>
    <!-- Page header -->
    <header>
        <nav>
            <ul>
                <li><a href="#home">Home</a></li>
                <li><a href="#about">About</a></li>
                <li><a href="#services">Services</a></li>
                <li><a href="#contact">Contact</a></li>
            </ul>
        </nav>
    </header>

    <!-- Main content -->
    <main>
        <!-- Hero section -->
        <section id="hero">
            <h1>Welcome to Our Website</h1>
            <p>Discover amazing content and services.</p>
        </section>

        <!-- Articles section -->
        <section id="articles">
            <h2>Latest Articles</h2>
            
            <article>
                <header>
                    <h3>Article Title</h3>
                    <p>Published on <time datetime="2024-01-15">January 15, 2024</time></p>
                </header>
                
                <p>Article content goes here...</p>
                
                <footer>
                    <p>Author: <cite>John Doe</cite></p>
                </footer>
            </article>
        </section>
    </main>

    <!-- Sidebar -->
    <aside>
        <h3>Related Links</h3>
        <ul>
            <li><a href="#">Link 1</a></li>
            <li><a href="#">Link 2</a></li>
        </ul>
    </aside>

    <!-- Page footer -->
    <footer>
        <p>&copy; 2024 Company Name. All rights reserved.</p>
        <address>
            Contact us at <a href="mailto:info@company.com">info@company.com</a>
        </address>
    </footer>
</body>
</html>
```

### Content Sectioning
```html
<!-- Main content area -->
<main>
    <!-- Standalone content -->
    <article>
        <h2>Blog Post Title</h2>
        <p>Blog post content...</p>
    </article>
    
    <!-- Thematic grouping -->
    <section>
        <h2>Products</h2>
        <p>Our product lineup...</p>
    </section>
</main>

<!-- Complementary content -->
<aside>
    <h3>Advertisement</h3>
    <p>Sidebar content...</p>
</aside>
```

### Text-Level Semantics
```html
<!-- Important content -->
<p>This is <strong>very important</strong> information.</p>

<!-- Emphasized content -->
<p>Please <em>carefully</em> read the instructions.</p>

<!-- Highlighted text -->
<p>The answer is <mark>42</mark>.</p>

<!-- Deleted and inserted text -->
<p>The price is <del>$50</del> <ins>$45</ins>.</p>

<!-- Abbreviations -->
<p><abbr title="HyperText Markup Language">HTML</abbr> is awesome!</p>

<!-- Keyboard input -->
<p>Press <kbd>Ctrl</kbd> + <kbd>C</kbd> to copy.</p>

<!-- Sample output -->
<p>The program outputs: <samp>Hello, World!</samp></p>

<!-- Variables -->
<p>The formula is: <var>E</var> = <var>mc</var><sup>2</sup></p>
```

### Time and Data
```html
<!-- Specific date and time -->
<p>The event starts at <time datetime="2024-12-25T09:00">December 25, 2024 at 9:00 AM</time>.</p>

<!-- Duration -->
<p>The movie is <time datetime="PT2H30M">2 hours and 30 minutes</time> long.</p>

<!-- Machine-readable data -->
<p>Price: <data value="29.99">$29.99</data></p>
```

---

## Characters & Symbols
HTML entities allow you to display special characters:

### Common HTML Entities
```html
<!-- Reserved characters -->
<p>Less than: &lt;</p>
<p>Greater than: &gt;</p>
<p>Ampersand: &amp;</p>
<p>Quote: &quot;</p>
<p>Apostrophe: &apos;</p>
<p>Non-breaking space: &nbsp;</p>

<!-- Copyright and trademark -->
<p>Copyright: &copy;</p>
<p>Registered: &reg;</p>
<p>Trademark: &trade;</p>

<!-- Currency symbols -->
<p>Euro: &euro;</p>
<p>Pound: &pound;</p>
<p>Yen: &yen;</p>
<p>Cent: &cent;</p>

<!-- Mathematical symbols -->
<p>Plus/minus: &plusmn;</p>
<p>Multiplication: &times;</p>
<p>Division: &divide;</p>
<p>Degree: &deg;</p>
<p>Infinity: &infin;</p>
<p>Square root: &radic;</p>

<!-- Arrows -->
<p>Left arrow: &larr;</p>
<p>Right arrow: &rarr;</p>
<p>Up arrow: &uarr;</p>
<p>Down arrow: &darr;</p>

<!-- Accented characters -->
<p>À á â ã ä å: &Agrave; &aacute; &acirc; &atilde; &auml; &aring;</p>
<p>È é ê ë: &Egrave; &eacute; &ecirc; &euml;</p>
<p>Ñ ñ: &Ntilde; &ntilde;</p>
```

### Numeric Character References
```html
<!-- Decimal notation -->
<p>Copyright: &#169;</p>
<p>Heart: &#9829;</p>

<!-- Hexadecimal notation -->
<p>Copyright: &#xA9;</p>
<p>Heart: &#x2665;</p>
```

---

## Meta Tags
Meta tags provide metadata about the HTML document:

### Essential Meta Tags
```html
<head>
    <!-- Character encoding -->
    <meta charset="UTF-8">
    
    <!-- Viewport for responsive design -->
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    
    <!-- Page description for search engines -->
    <meta name="description" content="A comprehensive guide to HTML basics and advanced techniques.">
    
    <!-- Keywords for search engines -->
    <meta name="keywords" content="HTML, web development, markup, tutorial">
    
    <!-- Author information -->
    <meta name="author" content="Your Name">
    
    <!-- Page title -->
    <title>HTML Guide - Learn Web Development</title>
</head>
```

### SEO and Social Media Meta Tags
```html
<head>
    <!-- Robots directive -->
    <meta name="robots" content="index, follow">
    
    <!-- Canonical URL -->
    <link rel="canonical" href="https://example.com/page">
    
    <!-- Open Graph (Facebook) -->
    <meta property="og:title" content="HTML Guide">
    <meta property="og:description" content="Learn HTML from basics to advanced">
    <meta property="og:image" content="https://example.com/image.jpg">
    <meta property="og:url" content="https://example.com/html-guide">
    <meta property="og:type" content="website">
    
    <!-- Twitter Cards -->
    <meta name="twitter:card" content="summary_large_image">
    <meta name="twitter:title" content="HTML Guide">
    <meta name="twitter:description" content="Learn HTML from basics to advanced">
    <meta name="twitter:image" content="https://example.com/image.jpg">
    
    <!-- Favicon -->
    <link rel="icon" type="image/x-icon" href="/favicon.ico">
    <link rel="apple-touch-icon" href="/apple-touch-icon.png">
</head>
```

### HTTP-Equiv Meta Tags
```html
<head>
    <!-- Refresh page -->
    <meta http-equiv="refresh" content="30">
    
    <!-- Redirect page -->
    <meta http-equiv="refresh" content="0; url=https://example.com">
    
    <!-- Content language -->
    <meta http-equiv="content-language" content="en-US">
    
    <!-- Cache control -->
    <meta http-equiv="cache-control" content="no-cache">
</head>
```

---

## CSS & JavaScript Integration
### CSS Integration
```html
<!-- External CSS (recommended) -->
<link rel="stylesheet" href="styles.css">
<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0/css/all.min.css">

<!-- Internal CSS -->
<style>
    body {
        font-family: Arial, sans-serif;
        background-color: #f0f0f0;
        margin: 0;
        padding: 20px;
    }
    
    .container {
        max-width: 1200px;
        margin: 0 auto;
    }
    
    h1 {
        color: #333;
        text-align: center;
    }
</style>

<!-- Inline CSS (avoid for maintainability) -->
<p style="color: red; font-weight: bold;">This text is red and bold.</p>
```

### JavaScript Integration
```html
<!-- External JavaScript (recommended) -->
<script src="script.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/jquery/3.6.0/jquery.min.js"></script>

<!-- Internal JavaScript -->
<script>
    // DOM manipulation
    document.addEventListener('DOMContentLoaded', function() {
        console.log('Page loaded');
        
        // Add event listeners
        document.getElementById('myButton').addEventListener('click', function() {
            alert('Button clicked!');
        });
    });
    
    // Function declarations
    function greetUser(name) {
        return `Hello, ${name}!`;
    }
</script>

<!-- Inline JavaScript (avoid for maintainability) -->
<button onclick="alert('Hello World!')">Click Me</button>
```

### Script Placement and Loading
```html
<!-- Defer script execution until DOM is parsed -->
<script src="script.js" defer></script>

<!-- Load and execute script asynchronously -->
<script src="analytics.js" async></script>

<!-- Load scripts in order -->
<script src="library.js"></script>
<script src="app.js"></script>

<!-- Module scripts -->
<script type="module" src="module.js"></script>
```

---

## Comments
HTML comments are ignored by browsers and help document code:

```html
<!-- Single line comment -->

<!-- 
Multi-line comment
spanning several lines
-->

<!-- TODO: Add navigation menu -->
<header>
    <!-- <nav>Navigation will go here</nav> -->
</header>

<!-- Conditional comments for Internet Explorer (deprecated) -->
<!--[if IE]>
    <p>You are using Internet Explorer</p>
<![endif]-->

<!-- Comment out sections during development -->
<!--
<section id="under-construction">
    <h2>Coming Soon</h2>
    <p>This section is under development.</p>
</section>
-->
```

**Best Practices:**
- Use comments to explain complex sections
- Document purpose of code blocks
- Leave TODO notes for future improvements
- Don't overuse comments for obvious code

---

## Emmet Shortcuts
Emmet helps write HTML faster with abbreviations:

### Basic Elements
```html
<!-- div -->
<div></div>

<!-- p -->
<p></p>

<!-- img -->
<img src="" alt="">
```

### Siblings and Children
```html
<!-- h1+h2+h3 -->
<h1></h1>
<h2></h2>
<h3></h3>

<!-- nav>ul>li -->
<nav>
    <ul>
        <li></li>
    </ul>
</nav>
```

### Multiplication
```html
<!-- ul>li*5 -->
<ul>
    <li></li>
    <li></li>
    <li></li>
    <li></li>
    <li></li>
</ul>

<!-- table>tr*3>td*4 -->
<table>
    <tr>
        <td></td>
        <td></td>
        <td></td>
        <td></td>
    </tr>
    <tr>
        <td></td>
        <td></td>
        <td></td>
        <td></td>
    </tr>
    <tr>
        <td></td>
        <td></td>
        <td></td>
        <td></td>
    </tr>
</table>
```

### Classes and IDs
```html
<!-- div.container -->
<div class="container"></div>

<!-- div.container.main -->
<div class="container main"></div>

<!-- div#main -->
<div id="main"></div>

<!-- div#header.container -->
<div id="header" class="container"></div>
```

### Attributes
```html
<!-- a[href="#" title="Link"] -->
<a href="#" title="Link"></a>

<!-- input[type="email" name="email" required] -->
<input type="email" name="email" required>

<!-- img[src="image.jpg" alt="Description" width="300"] -->
<img src="image.jpg" alt="Description" width="300">
```

### Text Content
```html
<!-- p{Hello World} -->
<p>Hello World</p>

<!-- a{Click here} -->
<a href="">Click here</a>

<!-- h1{Welcome}+p{This is a paragraph} -->
<h1>Welcome</h1>
<p>This is a paragraph</p>
```

### Numbering
```html
<!-- ul>li.item$*3 -->
<ul>
    <li class="item1"></li>
    <li class="item2"></li>
    <li class="item3"></li>
</ul>

<!-- div.section$$$*3 -->
<div class="section001"></div>
<div class="section002"></div>
<div class="section003"></div>
```

### Grouping
```html
<!-- (nav>ul>li*3)+footer -->
<nav>
    <ul>
        <li></li>
        <li></li>
        <li></li>
    </ul>
</nav>
<footer></footer>

<!-- div>(header>h1)+(main>section*2)+(footer>p) -->
<div>
    <header>
        <h1></h1>
    </header>
    <main>
        <section></section>
        <section></section>
    </main>
    <footer>
        <p></p>
    </footer>
</div>
```

### Complex Examples
```html
<!-- form>fieldset>legend{Contact Form}+(label[for="name"]{Name:}+input[type="text" id="name" name="name"])*3 -->
<form>
    <fieldset>
        <legend>Contact Form</legend>
        <label for="name">Name:</label>
        <input type="text" id="name" name="name">
        <label for="name">Name:</label>
        <input type="text" id="name" name="name">
        <label for="name">Name:</label>
        <input type="text" id="name" name="name">
    </fieldset>
</form>
```

---

## Best Practices

### Document Structure
- Use semantic HTML5 elements
- Include proper DOCTYPE declaration
- Set appropriate language attribute
- Use meaningful page titles

### Accessibility
- Always include `alt` attributes for images
- Use proper heading hierarchy (h1-h6)
- Associate labels with form controls
- Provide sufficient color contrast
- Use ARIA attributes when needed

### Performance
- Optimize images (use appropriate formats and sizes)
- Minimize HTTP requests
- Use external CSS and JavaScript files
- Implement lazy loading for images

### SEO
- Use semantic markup
- Include meta descriptions
- Optimize page titles
- Use structured data markup
- Create XML sitemaps

### Code Quality
- Validate HTML markup
- Use consistent indentation
- Write semantic, meaningful HTML
- Comment complex sections
- Follow naming conventions

### Example: Complete HTML5 Document
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta name="description" content="Professional web development services">
    <title>Web Development Services | Company Name</title>
    <link rel="stylesheet" href="styles.css">
    <link rel="icon" href="favicon.ico">
</head>
<body>
    <header>
        <nav aria-label="Main navigation">
            <ul>
                <li><a href="#home">Home</a></li>
                <li><a href="#services">Services</a></li>
                <li><a href="#about">About</a></li>
                <li><a href="#contact">Contact</a></li>
            </ul>
        </nav>
    </header>

    <main>
        <section id="home">
            <h1>Welcome to Our Website</h1>
            <p>Professional web development services.</p>
        </section>

        <section id="services">
            <h2>Our Services</h2>
            <article>
                <h3>Web Design</h3>
                <p>Custom website design and development.</p>
            </article>
        </section>
    </main>

    <aside>
        <h3>Latest News</h3>
        <ul>
            <li><a href="#">News item 1</a></li>
            <li><a href="#">News item 2</a></li>
        </ul>
    </aside>

    <footer>
        <p>&copy; 2024 Company Name. All rights reserved.</p>
        <address>
            Contact: <a href="mailto:info@company.com">info@company.com</a>
        </address>
    </footer>

    <script src="script.js"></script>
</body>
</html>
```

---

This comprehensive HTML guide covers everything from basic structure to advanced techniques. Practice these concepts to master HTML development!