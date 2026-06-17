function getStories() {
  return Promise.resolve(["Story 1", "Story 2"]);
}

function getFeed() {
  return Promise.reject("Feed Service Down");
}

function getSuggestions() {
  return Promise.resolve(["User A", "User B"]);
}

function getNotifications() {
  return Promise.resolve(5);
}

async function getHomePage() {

  const results = await Promise.allSettled([
    getStories(),
    getFeed(),
    getSuggestions(),
    getNotifications()
  ]);

  console.log(results);
}

getHomePage();
