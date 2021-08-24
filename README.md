# You Will Need Docker Installed

You can visit [the Docker Desktop page to download and install Docker](https://www.docker.com/products/docker-desktop) on your computer.

# Creating Our Project

Create a new empty folder anywhere on your computer. Place the `docker-compose.yml` file from this repository into the new empty folder you just created.

Point your command-line towards this new folder and run the `docker compose up -d` command. You only need to run this command this one time for the lifetime of this folder/project.

From now on, whenever you want to stop the app you can run `docker compose stop` and when you want to start it up again you can run `docker compose start`.

You can now visit `http://localhost:8080/` in a browser and should see a "Hello" message. Look further down on this page for information about all API Endpoints / URLs.

# Viewing The Database Directly

Your project folder now contains a folder named `db` which stores the actual database data.

You can visit [the official MongoDB Compass page](https://www.mongodb.com/try/download/compass) to download and install MongoDB Compass on your computer.

Once you've opened the MongoDB Compass app, you can use the following connection string to connect to our API's database:

`mongodb://root:root@localhost:27017/`

The `ourApp` database in particular is powering our API.

# API Endpoints

<details>
<summary>Register a New Account</summary>

**POST** `http://localhost:8080/register`

### Input

`{"username": "brad", "email": "example@example", "password": "qwertyqwerty"}`

### Output

Will return an array of reasons why registration failed (e.g. username is already taken, password must be at least 12 characters etc...). If successful, will return an object with the following properties: token, username, avatar.

</details>
<details>
<summary>Does Username Exist?</summary>

**POST** `http://localhost:8080/doesUsernameExist`

### Input

{"username": "barksalot"}

### Output

Returns `true` if the name is already taken; returns `false` if the name is available.

</details>
<details>
<summary>Does Email Exist?</summary>

**POST** `http://localhost:8080/doesEmailExist`

### Input

{"email": "barksalot@example.com"}

### Output

Returns `true` if the email is already in use; returns `false` if the email has not been used yet.

</details>
<details>
<summary>Login</summary>

**POST** `http://localhost:8080/login`

### Input

`{"username": "barksalot", "password": "qwertyqwerty"}`

### Output

Failed login attempts simply return false. If successful, will return an object with the following properties: token, username, avatar.

</details>
<details>
<summary>Create New Post</summary>

**POST** `http://localhost:8080/create-post`

### Input

`{"title": "Being a Dog is Wonderful", "body": "Bark bar bark lorem ipsum. Lorem ipsum.", "token": "yourValidTokenValueHere"}`

### Output

Will return an array of reasons why post creation failed (e.g. You must provide a title, etc..."). If successful, returns the ID value of the newly created post.

</details>
<details>
<summary>View Single Post</summary>

**GET** `http://localhost:8080/post/6122b229996cb5001293d2c4`

### Output

Will return false if post does not exist. If post exists this endpoint will return an object with the following example shape:

```
{
  "_id": "6122b229996cb5001293d2c4",
  "title": "My New Post Today",
  "body": "Lorem ipsum dolor sit amet lorem ipsum.",
  "createdDate": "2021-08-22T20:23:05.653Z",
  "author": {
    "username": "barksalot",
    "avatar": "https://gravatar.com/avatar/b9216295c1e3931655bae6574ac0e4c2?s=128"
  },
  "isVisitorOwner": false
}
```

</details>
<details>
<summary>Edit / Update An Existing Post</summary>

**POST** `http://localhost:8080/post/6122b3ea996cb5001293d2c7/edit`

### Input

```
{"token": "yourValidTokenHere", "title": "Being a Dog is Great!!!!!", "body": "Woof woof"}
```

### Output

Will return "success" or "failure" accordingly.

</details>
<details>
<summary>Delete An Existing Post</summary>

**DELETE** `http://localhost:8080/post/6122b50e996cb5001293d2c9`

### Input

```
{"token": "yourValidTokenHere"}
```

### Output

Will return "success" or "You do not have permission to perform that action." accordingly.

</details>
<details>
<summary>Start Following a User</summary>

**POST** `http://localhost:8080/addFollow/barksalot`

### Input

```
{"token": "yourValidTokenHere"}
```

### Output

Will return true if successful. Will return false if you try to follow yourself or a user you are already following.

</details>
<details>
<summary>Stop Following a User</summary>

**POST** `http://localhost:8080/removeFollow/barksalot`

### Input

```
{"token": "yourValidTokenHere"}
```

### Output

Will return true if successful. Will return false if you try to stop following someone you are not currently following.

</details>
<details>
<summary>Get Feed of Posts From Users You Follow</summary>

**POST** `http://localhost:8080/getHomeFeed`

### Input

```
{"token": "yourValidTokenHere"}
```

### Output

Will return an array of posts from the users you follow, for example:

```
[
  {
    "_id": "6122b3ea996cb5001293d2c7",
    "title": "Being a Dog is Great!!!!!",
    "body": "Woof woof",
    "createdDate": "2021-08-22T20:30:34.387Z",
    "author": {
      "username": "barksalot",
      "avatar": "https://gravatar.com/avatar/b9216295c1e3931655bae6574ac0e4c2?s=128"
    },
    "isVisitorOwner": false
  },
  {
    "_id": "6122b229996cb5001293d2c4",
    "title": "My New Post Today",
    "body": "Lorem ipsum!",
    "createdDate": "2021-08-22T20:23:05.653Z",
    "author": {
      "username": "barksalot",
      "avatar": "https://gravatar.com/avatar/b9216295c1e3931655bae6574ac0e4c2?s=128"
    },
    "isVisitorOwner": false
  }
]
```

</details>
<details>
<summary>General Profile Info (Counts etc...)</summary>

**POST** `http://localhost:8080/profile/barksalot`

### Input

```
{"token": "yourValidTokenHere"}
```

### Output

Will return information about the requested user account. You must send your token along so the system knows if you are already following the user or not. Here is an example of the object shape it returns:

```
{
  "profileUsername": "barksalot",
  "profileAvatar": "https://gravatar.com/avatar/b9216295c1e3931655bae6574ac0e4c2?s=128",
  "isFollowing": true,
  "counts": {
    "postCount": 2,
    "followerCount": 1,
    "followingCount": 0
  }
}
```

</details>
<details>
<summary>Profile - All Posts by Author</summary>

**GET** `http://localhost:8080/profile/barksalot/posts`

### Output

Will return an array with posts by the requested user.

```
[
  {
    "_id": "6122b3ea996cb5001293d2c7",
    "title": "Being a Dog is Great!!!!!",
    "body": "Woof woof",
    "createdDate": "2021-08-22T20:30:34.387Z",
    "author": {
      "username": "barksalot",
      "avatar": "https://gravatar.com/avatar/b9216295c1e3931655bae6574ac0e4c2?s=128"
    },
    "isVisitorOwner": false
  },
  {
    "_id": "6122b229996cb5001293d2c4",
    "title": "My New Post Today",
    "body": "Lorem ipsum!",
    "createdDate": "2021-08-22T20:23:05.653Z",
    "author": {
      "username": "barksalot",
      "avatar": "https://gravatar.com/avatar/b9216295c1e3931655bae6574ac0e4c2?s=128"
    },
    "isVisitorOwner": false
  }
]
```

</details>
<details>
<summary>Profile - List Followers For a User</summary>

**GET** `http://localhost:8080/profile/barksalot/followers`

### Output

Will return an array of the users who follow this user.

```
[
  {
    "username": "brad",
    "avatar": "https://gravatar.com/avatar/b9408a09298632b5151200f3449434ef?s=128"
  },
  {
    "username": "meowsalot",
    "avatar": "https://gravatar.com/avatar/basdfasfadsf32b5151200f3449434ef?s=128"
  }
]
```

</details>
<details>
<summary>Profile - List The Users This User Follows</summary>

**GET** `http://localhost:8080/profile/brad/following`

### Output

Will return an array of the users who this user follows.

```
[
  {
    "username": "barksalot",
    "avatar": "https://gravatar.com/avatar/b9216295c1e3931655bae6574ac0e4c2?s=128"
  },
  {
    "username": "meowsalot",
    "avatar": "https://gravatar.com/avatar/basdfasfadsf32b5151200f3449434ef?s=128"
  }
]
```

</details>
<details>
<summary>Search</summary>

**POST** `http://localhost:8080/search`

### Input

{"searchTerm": "dog"}

### Output

Returns an array of posts that include the term you searched for.

```
[
  {
    "_id": "6122b3ea996cb5001293d2c7",
    "title": "Being a Dog is Great!!!!!",
    "body": "Woof woof",
    "createdDate": "2021-08-22T20:30:34.387Z",
    "author": {
      "username": "barksalot",
      "avatar": "https://gravatar.com/avatar/b9216295c1e3931655bae6574ac0e4c2?s=128"
    },
    "isVisitorOwner": false
  }
]
```

</details>

###### &nbsp;

# Go Practice and Create!

You will run into many challenges along the way, but you now have a real project to sink your teeth into. With enough Googling, I am confident you can program your own front-end user-interface / proxy server that communicates with this API to create an enjoyable experience for hypothetical users.

Maybe you want to create a single page application using React or Vue.js. Or maybe you want to have traditional full-page reload / submits using a proxy server powered by PHP or Python. Maybe you want to create an iOS or Android app that communicates with this API. The choice is up to you!
