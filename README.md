Imagine you are working for an online artist platform that hosts various art competitions.Each competition needs a random selection of judges from a larger pool of available judges.For example, you might want 5 random judges out of a pool ranging from Judge 1 to Judge 20.
You have been provided with the following utility function
get_random_items_from_range, which helps in generating such randomized selections:

```js
const get_random_items_from_range = (
  items = 1,
  start = 0,
  end = 1,
  step = 1
) => {
  // Function implementation here...
};
```

Task:

- Create the inner workings of this function based on your understanding.
- Your function should:
  1. Generate an array representing a range of numbers.
  2. Randomly select numbers from this range.
  3. Ensure error handling if more items are requested than available in the range.
     TEST DATA::
     Given examples of random data may not be the same in your case.
     Range: [1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19]
     Random 5: [3, 8, 7, 13, 11]
     get_random_items_from_range(5, 1, 20, 1);
  - Out of bounds, throws error.
  - Only 4 numbers are possible
    get_random_items_from_range(5, 10, 30, 5);
  - Returns only 4 numbers in random order .e.g. [10, 15, 25, 20]
    get_random_items_from_range(4, 10, 30, 5);
  - Gives 9 numbers out of 13 in random order.e.g. [194, 86, 62, 122, 146, 98, 134, 170, 110]
    get_random_items_from_range(9, 50, 200, 12);

## Solution

#### BDD

Scenario: Successfully getting random items within the range
Given the range starts at 1 and ends at 20 with a step of 1
When I request 5 random items
Then I should receive 5 items within the range 1 to 19

Scenario: Requesting more items than available in the range
Given the range starts at 10 and ends at 30 with a step of 5
When I request 5 random items
Then I should receive an error indicating only 4 items are available

Scenario: Requesting the exact number of available items
Given the range starts at 10 and ends at 30 with a step of 5
When I request 4 random items
Then I should receive 4 items within the range 10, 15, 20, 25

Scenario: Randomly selecting a large number of items from a large range
Given the range starts at 50 and ends at 200 with a step of 12
When I request 9 random items
Then I should receive 9 items within the range of numbers generated by the step of 12 from 50 to 194

#### Pseudocode

```js
// Function get_random_items_from_range(items, start, end, step)
// Initialize an empty list called range

// // Generate the range of numbers
// For each number i starting from start to less than end, incrementing by step
// Add i to the range list

// // Check if the requested number of items exceeds the available range size
// If the number of items requested is greater than the length of the range list
// Print an error message "Requested number of items exceeds available range."
// Exit the function

// // Shuffle the range to randomize the selection
// Initialize an empty list called shuffled
// While there are elements in the range list
// Randomly select an element from the range list
// Remove the selected element from the range list
// Add the selected element to the shuffled list

// // Select the requested number of items from the shuffled list
// Initialize an empty list called selected_items
// For each of the first 'items' elements in the shuffled list
// Add the element to the selected_items list

// // Return the selected items
// Return selected_items
// End Function
```

```js
const get_random_items_from_range = (
  items = 1,
  start = 0,
  end = 1,
  step = 1
) => {
  // Generate the range of numbers
  const range = [];
  for (let i = start; i < end; i += step) {
    range.push(i);
  }

  // Check if the requested number of items exceeds the available range size
  if (items > range.length) {
    throw new Error(
      `Requested ${items} items, but only ${range.length} items are available in the range.`
    );
  }

  // Shuffle the range array to randomize item selection
  // const shuffled = range.sort(() => 0.5 - Math.random());

  // Fisher-Yates shuffle
  for (let i = range.length - 1; i > 0; i--) {
    const j = Math.floor(Math.random() * (i + 1));
    [range[i], range[j]] = [range[j], range[i]];
  }

  // Select the first 'items' elements from the shuffled array
  return shuffled.slice(0, items);
};

// Test cases
console.log(get_random_items_from_range(5, 1, 20, 1)); // Random 5 numbers from 1 to 19
console.log(get_random_items_from_range(5, 10, 30, 5)); // Error: Only 4 numbers are possible
console.log(get_random_items_from_range(4, 10, 30, 5)); // Returns 4 numbers in random order
console.log(get_random_items_from_range(9, 50, 200, 12)); // Gives 9 numbers out of 13 in random order
```
