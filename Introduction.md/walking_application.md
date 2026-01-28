# Walking An Application
#


## MISSION

- View Source - Use your browser to view the human-readable source code of a website.

- Inspector - Learn how to inspect page elements and make changes to view usually blocked content.

- Debugger - Inspect and control the flow of a page's JavaScript

- Network - See all the network requests a page makes.

##

1. View Page Source

- For the first task, I accessed the website and used Right Click View Page Source.

- This allowed me to examine the raw HTML, CSS, and JavaScript as delivered by the server. I navigated through:

- HTML structure

- Embedded and external JavaScript files

- CSS stylesheets

- Developer comments within the code

- The goal was to identify any potential clues, comments, hardcoded values, or credentials that developers may have unintentionally left in the source code. This step provides insight into how the webpage is structured before any client-side interaction occurs.

##

2. Inspector (Elements Tool)

- While viewing the page source is useful, it does not reflect dynamic changes that occur when users interact with the webpage (such as clicking buttons or submitting forms). To observe these real-time changes, the Inspector tool is required.

- For this task, I used Right Click → Inspect --> Using the Inspector, I was able to --> Observe changes to the DOM as I interacted with the page  -->  Identify which elements were shown, hidden, or modified dynamically --> Understand how CSS and JavaScript affect page behavior in real time --> This tool is essential for analyzing interactive web applications where content changes after the initial page load.

##

3. Debugger (JavaScript Analysis)

- The Debugger is used to inspect, analyze, and control JavaScript execution.

- Many websites use minified or compressed JavaScript files to reduce file size and make the code harder to read. In the Sources / Debugger panel, clicking the {} (Pretty Print) button reformats the code into a readable structure.

- For this task:

- I navigated to Inspect → Debugger

- Opened a JavaScript file (e.g., flash.js or another relevant .js file)

- Set breakpoints on specific lines of code

- By placing breakpoints, I was able to pause execution and prevent certain lines from running during page refresh or interaction. This helped me understand the logic flow and behavior of the application’s client-side scripts.

##

4. Network Panel

- The Network panel is used to monitor all requests made by the webpage, including communication with external servers.

- For this task:

- I opened Inspect → Network

- Interacted with the website’s contact form

- Entered random (gibberish) data into the form

- Submitted the form and refreshed the page

- In the Network tab, I observed:

- A new network request generated upon form submission

- A document or request showing the data being sent

- The destination endpoint receiving the message

- This confirmed how the website transmits user input to an external server or backend service and demonstrated the importance of the Network panel for understanding data flow and request handling.