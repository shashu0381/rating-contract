# ProductRating Smart Contract

## Overview

The `ProductRating` smart contract allows users to record ratings for products and provides functionality to retrieve rating descriptions and check the acceptability of a product based on its rating.

## Features

1. **Record Product Rating:** Users can record their product ratings.
2. **Get Rating Description:** Provides a description based on the rating.
3. **Check Product Acceptability:** Checks if a product is acceptable based on its rating.

## Contract Details

### Mappings
- `productRatings`: Maps user addresses to their product ratings.

### Functions

- **recordRating(uint _rating):**
  Records the rating for the product given by the user. It uses an `assert` statement to ensure the rating is between 0 and 5.
  
  ```solidity
  function recordRating(uint _rating) public {
      assert(_rating >= 0);
      productRatings[msg.sender] = _rating;
  }
  ```

- **getRatingDescription(uint rating):**
  Returns a description based on the provided rating. The function uses a `require` statement to ensure the rating is between 0 and 5.
  
  ```solidity
  function getRatingDescription(uint rating) public pure returns (string memory) {
      require(rating >= 0 && rating <= 5, "Rating must be between 0 and 5");
  
      if (rating == 5) {
          return "Excellent";
      } else if (rating == 4) {
          return "Good";
      } else if (rating == 3) {
          return "Average";
      } else if (rating == 2) {
          return "Below Average";
      } else {
          return "Poor";
      }
  }
  ```

- **checkAcceptability():**
  Checks if the recorded rating for the user is acceptable (rating of 3 or above). Uses a `revert` statement to indicate that the rating is not acceptable if it is below 3.
  
  ```solidity
  function checkAcceptability() public view returns (string memory) {
      uint rating = productRatings[msg.sender];
      require(rating >= 0 && rating <= 5, "Rating should be between 0 and 5");
  
      if (rating >= 3) {
          return "Acceptable";
      } else {
          revert("Not Acceptable: minimum rating to be acceptable is 3");
      }
  }
  ```

## Usage

### Deployment
Deploy the contract to an Ethereum network using a development framework like Hardhat or Truffle.

### Interacting with the Contract

#### Recording a Rating
To record a product rating, call the `recordRating` function with the desired rating.

```solidity
recordRating(4);
```

#### Getting Rating Description
To get the description of a rating, call the `getRatingDescription` function with the rating.

```solidity
string memory description = getRatingDescription(4); // Returns "Good"
```

#### Checking Product Acceptability
To check if the product rating is acceptable, call the `checkAcceptability` function.

```solidity
string memory acceptability = checkAcceptability(); // Returns "Acceptable" if rating >= 3, reverts otherwise
```

## Error Handling

- The `recordRating` function uses `assert` to ensure the rating is non-negative.
- The `getRatingDescription` function uses `require` to ensure the rating is between 0 and 5.
- The `checkAcceptability` function uses `require` to ensure the rating is between 0 and 5, and `revert` to indicate the rating is not acceptable if it is below 3.

## License
This project is licensed under the MIT License.
