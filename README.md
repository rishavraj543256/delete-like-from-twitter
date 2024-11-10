
# Twitter Unliker Script

This repository contains a JavaScript script that automates the process of unliking tweets from your Twitter account. The script continuously unlikes tweets in bulk by simulating user interactions with the "Unlike" button, scrolling to load more tweets, and respecting rate limits by introducing wait times between actions.



## Features

- Automatically unlikes up to 5000 tweets at a time.
- Handles scrolling to load more tweets automatically.
- Pauses and resumes to prevent being rate-limited by Twitter.
- Easy to use via the browser's Developer Tools console.


## Requirements
- A Twitter account with tweets that you’ve liked.
- A desktop web browser (e.g., Google Chrome, Firefox).
- Basic familiarity with browser developer tools.
## How to Use
- Open Twitter in Your Web Browser
- Navigate to Twitter and log in to your account.

- Right-click anywhere on the page and select Inspect (or press Ctrl+Shift+I in most browsers) to open the Developer Tools panel.

- In the Developer Tools, click on the Console tab to access the JavaScript console.

- Copy the unliking script provided in this repository and paste it into the Console.

- Press Enter to run the script. It will start unliking tweets automatically. The script logs the progress in the console, including how many tweets have been unliked.

- The script will unlike tweets in batches, and if it hits a rate limit, it will pause for 20 seconds and then resume automatically.

- To stop the script, simply close the browser tab or refresh the page.
## Usage/Examples

```javascript
// Function to find the "Unlike" button
function nextUnlike() {
  return document.querySelector('[data-testid="unlike"]');
}

// Function to wait for a specific amount of time
function wait(ms) {
  return new Promise(resolve => setTimeout(resolve, ms));
}

// Function to unlike posts faster
async function removeAll() {
  let count = 0;
  let next = nextUnlike();
  
  while (next && count < 5000) {
    try {
      next.focus();
      next.click();
      console.log(`Unliked ${++count} tweets`);
      
      // Reducing wait time to 2 seconds to make it faster
      await wait(2000);
      
      next = nextUnlike();
      
      if (!next && count < 5000) {
        // Scroll to load more tweets when no unlike button is found
        window.scrollTo(0, document.body.scrollHeight); // Scroll to the bottom to load more tweets
        await wait(5000); // Reduced wait time for scrolling to load new tweets
        next = nextUnlike(); // Get the next unlike button
      }
    } catch (error) {
      console.error('An error occurred:', error);
      break;
    }
  }

  // If no more unlike buttons, restart after short pause
  if (!next) {
    console.log('No more tweets to unlike or rate-limit reached. Pausing...');
    await wait(20000); // Reduced pause time to 20 seconds before checking again
    removeAll(); // Automatically restart the function after the wait time
  } else {
    console.log('Finished, count =', count);
  }
}

// Start the unliking process
removeAll();

```


## License

This project is licensed under the MIT License - see the LICENSE file for details. [MIT](https://choosealicense.com/licenses/mit/)
