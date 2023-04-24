---
title: "How to install & run MongoDB shell on Mac Terminal?"
seoTitle: "Install & Run MongoDB Shell on Mac"
datePublished: Mon Apr 24 2023 15:34:07 GMT+0000 (Coordinated Universal Time)
cuid: clgv00pbl000209kz6cz26uns
slug: how-to-install-and-run-mongodb-shell-on-mac-terminal
cover: https://cdn.hashnode.com/res/hashnode/image/stock/unsplash/cijiWIwsMB8/upload/439bb9bdb56f416f395bffa48953bf78.jpeg
tags: javascript, mongodb, databases, webdev

---

## Installation

* To install MongoDB you will need homebrew which is a package manager for Mac, something similar to npm or yarn.
    
* To install homebrew, head to [homebrew installation](https://brew.sh/) and follow the steps to install it.
    
* Once you have successfully installed you can verify the installation process whether homebrew is installed or not by using the following command.
    

```bash
brew -v
```

* Now we can start our MongoDB installation.
    
* Head to the terminal and type the following command, press enter, and sit back and relax.
    

```bash
brew install mongodb-community@6.0
```

## Running of MongoDB Instance

* Once the installation is complete, you can start the MongoDB service by using the following command.
    

```bash
brew services start mongodb-community
```

* To open the MongoDB Shell use the following command.
    

```bash
mongosh
```

* And you can start working on your database.
    

* Make sure you stop the service once you are done with it, using the following command.
    

```bash
brew services stop mongodb-community
```

This is the simplest way to get started with MongoDB on mac. But I would highly recommend downloading MongoDB Compass(GUI).

---

See you in the next one 🫡, till then you can follow me on [@twitter](https://twitter.com/bharathkalyans) & [@github](https://github.com/bharathkalyans/).