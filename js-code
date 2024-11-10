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
