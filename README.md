
# FoliaRunnable

**FoliaRunnable** is a simple Java class that helps developers schedule and manage tasks on Folia-based Minecraft servers. It unifies different scheduling contexts (asynchronous, entity-based, global region, or specific region) into a single API, making your code cleaner and easier to maintain.

## Table of Contents

1. [Overview](#overview)  
2. [Features](#features)  
3. [Usage](#usage)  
4. [Contributing](#contributing)  

---

## Overview

Folia is an experimental fork of Paper that aims to introduce a multi-threaded scheduler. This project provides a single Java file, `FoliaRunnable.java`, which you can incorporate directly into your plugin. Once added, you can schedule tasks using the provided APIs without having to write repetitive scheduling code for each type of scheduler (`AsyncScheduler`, `EntityScheduler`, `GlobalRegionScheduler`, and `RegionScheduler`).

---

## Features

- **Supports All Folia Schedulers**  
  Schedule tasks asynchronously, or by entity, global region, or a specific region (location-based or chunk-based).

- **Unified API**  
  A single class, `FoliaRunnable`, handles different scheduling contexts—no need to write multiple classes for each scheduler type.

- **Easy to Extend**  
  Just extend `FoliaRunnable` and override `run()`. Then choose how you want to schedule it (e.g., run once, run delayed, or run at a fixed rate).

---

## Usage

1. **Copy `FoliaRunnable.java` into your plugin**  
   Simply download the file from this repository and place it in your plugin’s package structure. You do not need any additional build tools or dependencies, as it only uses the Folia (Paper) scheduler interfaces.

2. **Create a custom task**  
   ```java
   public class MyFoliaTask extends FoliaRunnable {

       public MyFoliaTask(AsyncScheduler scheduler) {
           // Example for async scheduling (with no time unit if not needed):
           super(scheduler, null);
       }

       @Override
       public void run() {
           // This code is executed when the task runs
           System.out.println("Hello from MyFoliaTask!");
       }
   }
   ```

3. **Schedule the task** (e.g., in your main plugin class):
   ```java
   public class MyPlugin extends JavaPlugin {

       @Override
       public void onEnable() {
           // Example using an asynchronous scheduler
           AsyncScheduler asyncScheduler = getServer().getAsyncScheduler();

           MyFoliaTask task = new MyFoliaTask(asyncScheduler);
           
           // Run once as soon as possible
           task.run(this);

           // Or run with a delay of 20 ticks
           // task.runDelayed(this, 20);

           // Or schedule at a fixed rate
           // task.runAtFixedRate(this, 20, 100);
       }
   }
   ```
4. **Check or cancel the task**  
   - Use `task.isCancelled()` to see if it’s still active.  
   - Use `task.cancel()` to terminate future runs.  

That’s it! You can adapt this approach for entity-based tasks, global tasks, or region-based tasks, depending on your needs.

