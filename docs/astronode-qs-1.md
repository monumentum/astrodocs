---
id: astronode-qs-1
title: Astronode Installation
sidebar_label: Installation
---

This quick start guide is based on **M E N** of famous **MEAN** structure. Our early project is totally based on **Express Framework** and **Mongoose ORM** so we will use our own **plugins** in this guide, remember to read the other plugin's documentation during implementation time!

## Install Astronode

First of all you need to init your project as an usual node program, after it you will use the command
`npm install --save astronode`

## Installing Astronode Plugins

By default astronode expect 2 base plugins:
    - **Data Plugin:** This one will take controll of all data environment.
    - **Engine Plugin:** This one will take controll of routes and middlewares.

So let execute the command: `npm install --save astronode-express-plugin astronode-mongoose-plugin`, next step is touch and understand some basic stuff from out `astronode.config.json`