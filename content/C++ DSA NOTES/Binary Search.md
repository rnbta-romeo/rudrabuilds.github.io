---

---

When we search for a word in dictionary, we don't look for it page by page. That's a long and hefty process. We instead open up a random page, and then relative to the first letter of the word we decide whether to search before the page we have opened or after that page. 
That's exactly how binary search works, instead of checking every element in the array to find the target, we first find the middle of the array and compare with the target. Note that binary search can only be applied in a sorted array. If the mid value in smaller that the target then we search in the left half of the array and mid value is greater than the target then we search in the right half of the array.