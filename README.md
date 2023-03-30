# User Table

## The following functionality is implemented on the client side:
```
- Ability to search users by username or e-mail address

- Ability to sort the table by the following fields: date of registration, rating

- Implemented pagination

- Implemented the ability to remove a user from the list
```

## Functional description
```
- The list of users is obtained using a GET request

- When you click on the "Registration Date" in the sorting, the list is sorted by registration date
from the largest date to the smallest when you click on the sorting field again.
dates, the list is sorted by date from the smallest date to the largest.

- When you click on "Progress" in sorting, the list is sorted by the "Progress" field in
ascending/descending order, similar to the "Registration Date" field.

- The clear filter button appears when the search is active or after sorting by any of the fields is applied

- When you click on the button, the search and sort filter is reset
```

### The response is a list of users with the following fields:
```
— id – Unique user number

— login – User name

— email – the user's email address

— registration_date – registration date

— rating – user rating
```


# Link: https://sergeyw1.github.io/user-table/
