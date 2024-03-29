# Frdg.xyz
**What exactly is in this container, and when did I stick it in the fridge?**

## What is it?
This is a working prototype of a system designed to track leftovers in the fridge quickly and easily.

The system works using a combination of QR code labels and a Svelte webapp hosted on Firebase. When storing leftovers, users scan the label on its container and briefly describe the contents using a short title field and a series of emoji labels to indicate whether it contains food sensitivity triggers.

When pulling something out of the fridge, the user can scan the QR code to view the date the item was stored as well as its content.

## Why was it built?
I live with 2 other people who sometimes share food, and we found ourselves throwing out a lot of leftovers simply because nobody could remember when exactly everything was cooked, and what leftovers were safe for whom (based on their ingredients). I knew from experience that the system had to be fast and intuitive or nobody would use it. QR codes were chosen so that we could use our smartphones' native camera app to perform the scanning - a quick and easy entry point.

Emojis were chosen for the ingredient tags as they are generally well-designed and clear in their meaning, and using them rather than custom icons reduced the overhead of the site and simplified the design.

In the end, we discovered that water-soluble labels (like these: https://www.messbrands.com/product/dissolvable-food-labels-white-blank/) were a better fit because they were faster and more frictionless. However, I'm still thrilled with the design and had a blast making it!

## How far along is it?
I'd describe it as a "functional prototype". Users can scan labels to open the webapp to the appropriate entry, and can create and view leftover entries.

Initializing the backend is still very much a manual process - label IDs must be entered into the database directly in order to be used. There's also nothing built to assist with label printing, so finding a way to print the IDs as scannable QR code labels is up to the end user.

## Screenshots
### New item
![image](https://github.com/phillmp/kitchenapp/assets/8772256/5f543a8a-1e6c-4edf-bb07-200bb33ee5d1)

### Saving new leftovers 
![image](https://github.com/phillmp/kitchenapp/assets/8772256/fd211322-7564-49b1-8da2-e53fd99848c8)

### Viewing existing leftovers
![image](https://github.com/phillmp/kitchenapp/assets/8772256/eb663ec4-0f43-49b4-ab3c-10a5c4cb5035)

### Example leftover container lid
![image](https://github.com/phillmp/kitchenapp/assets/8772256/d2ab1e5f-d198-4d5f-9d43-c6cb6d169606)






Copyright 2021 Phil Morris-Parent
All Rights Reserved
