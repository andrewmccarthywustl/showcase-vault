---
cssclasses:
  - no_line_width
  - document--no-title
  - home-tasks
---

```dataviewjs
// Create a container div for the clock widget
const clockDiv = this.container.createDiv({ cls: "clock-widget" });
// Inject HTML structure after the container is created
clockDiv.innerHTML = `
  <div style="text-align: center;">
      <p id="clock-date" style="margin: 0; font-weight: bold; font-size: 1.2em;">Loading...</p>
      <h1 id="clock-time" style="font-size: 3em; margin: 0;">Loading...</h1>
  </div>
`;
// Cache the date and time elements outside the update function for performance
const dateElement = clockDiv.querySelector("#clock-date");
const clockElement = clockDiv.querySelector("#clock-time");

// JavaScript function to update the clock and date
function updateClock() {
  const now = new Date();

  // Get the day of the week and format the date
  const dayOfWeek = now.toLocaleDateString('en-US', { weekday: 'long' }); // e.g., "monday"

  // Get the day with the appropriate suffix (1st, 2nd, 3rd, etc.)
  const day = now.getDate();
  let daySuffix = "th";
  if (day % 10 === 1 && day !== 11) {
    daySuffix = "st";
  } else if (day % 10 === 2 && day !== 12) {
    daySuffix = "nd";
  } else if (day % 10 === 3 && day !== 13) {
    daySuffix = "rd";
  }

  // Get the month name
  const month = now.toLocaleDateString('en-US', { month: 'long' });

  // Format the date string as "monday, march 31st"
  const dateString = `${dayOfWeek}, ${month} ${day}${daySuffix}`;

  // Specify options to include AM/PM in the time format
  let timeString = now.toLocaleTimeString([], {
    hour: '2-digit',
    minute: '2-digit',
    hour12: true // This ensures AM/PM is included
  });

  // Force AM/PM to be uppercase
  timeString = timeString.toUpperCase();

  // Update the content of the date and clock directly
  if (dateElement) {
    dateElement.textContent = dateString;
  }
  if (clockElement) {
    clockElement.textContent = timeString;
  }

  // Schedule the next update
  requestAnimationFrame(updateClock);
}

// Start updating the clock
updateClock();
```

> [!nav-bar] 
> [![[attachments/infinity-note-icon.svg]]](infinity-note.md)
> [![[journal-icon.svg]]](journal/table-of-contents-journal.md) 
> [![[goals-icon.svg]]](goals.md) 
> [![[media-icon.svg]]](media-list.md) 
> [![[personal-website-icon.svg]]](https://www.youtube.com/watch?v=dQw4w9WgXcQ)

> [!nav-bar] 
> [![[claude-icon.svg]]](file:///Applications/Claude.app) 
> [![[gemini-icon.svg]]](https://gemini.google.com/app) 
> [![[ai-studio-icon.svg]]](https://aistudio.google.com/prompts/new_chat) 
> [![[notebook-lm-icon.svg]]](https://notebooklm.google.com/) 
> [![[chat-gpt-icon.svg]]](https://chatgpt.com/)

> [!nav-bar] 
>[![[class-icon.svg]]](table-of-contents-class.md)

---oldest tasks---

```dataview
TASK
FROM "infinity-note.md"
WHERE !completed
SORT line DESC
LIMIT 5
```

---most rectent tasks ---

```dataview
TASK
FROM "infinity-note.md"
WHERE !completed
LIMIT 5
```

---goals---

```dataview
TASK
FROM "goals.md"
WHERE !completed
LIMIT 5
```

<p class="quote">"What have I done today to make someone's life a little better?" - G Mc</p> (I have not implemented the styling in yet for the quote or the button)

```dataviewjs
// Create a button that will display a random task when clicked
const container = this.container
const buttonDiv = container.createEl('div')
const randomTaskButton = buttonDiv.createEl('button')
randomTaskButton.textContent = "Random Movie"

// Create a div to hold the result
const resultDiv = container.createEl('div', {cls: 'result-container'})

randomTaskButton.addEventListener('click', async () => {
    try {
        // Get all tasks from the media-list.md file
        const allTasks = await dv.pages('"media-list"').file.tasks
                       .where(t => !t.completed)
                       .values

        if (allTasks.length > 0) {
            // Select a random task
            const randomIndex = Math.floor(Math.random() * allTasks.length)
            const randomTask = allTasks[randomIndex]

            // Display the random task
            resultDiv.innerHTML = `<p><strong>Random Movie:</strong> ${randomTask.text}</p>`

            // Add the source if available
            if (randomTask.link && randomTask.link.path) {
            }
        } else {
            resultDiv.innerHTML = "<p>No tasks found! Make sure your media-list.md file contains tasks formatted as '- [ ] Task text'</p>"
        }
    } catch (error) {
        resultDiv.innerHTML = `<p>Error: ${error.message}</p>`
        console.error(error) // This will show in the developer console for debugging
    }
})
```
